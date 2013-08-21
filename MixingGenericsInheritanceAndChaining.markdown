# Mixing Generics, Inheritance and method chaining

In my last post on unit testing, I had written about a technique I'd learnt about [simplifying test set ups with the builder pattern](http://niftybits.wordpress.com/2013/08/14/unit-tests-simplifying-test-setup-with-builders/) that can be used to provide a higher level API resulting in [DAMP](http://blog.jayfields.com/2006/05/dry-code-damp-dsls.html) tests.

We have MVC4 controllers that deal with logged in user derive from `TenantController` which provides `TenantId` and `SubscriptionId` properties. for ex:

```cs
	class EventsController : Controller 
	{
		public ActionResult Post (MyModel model) 
		{
		// access request, form and other http things
		}
	}

	class TenantController: Controller 
	{
		public Guid TenantId {get; set;}
		public Guid SubscriptionId {get; set;}
	}

	class TaskController: TenantController
	{
		public ActionResult GetTasks()
		{
			// Http things and most probably tenantId and subId as well.
		}
	}
```

So, tests for `EventsController` will require HTTP setup (request content, headers etc) where as for anything deriving from `TenantController` we also need to be able to set up things like `TenantId`.

### Builder API
Let's start from how we'd like our API to be. So, for something that just requires HTTP context, we'd like to say:

```cs
	controller = new EventsControllerBuilder()
                .WithConstructorParams(mockOpsRepo.Object)
                .Build();
```

And for something that derives from `TenantController`:

```cs
	controller = new TaskControllerBuilder()
                .WithConstructorParams(mockOpsRepo.Object)
                .WithTenantId(theTenantId)
                .WithSubscriptionId(theSubId)
                .Build();
```
The controller builder will basically keep track of the different options and always return `this` to facilitate chaining. Apart from that, it has a `Build` method which builds a `Controller` object according to the different options and then returns the controller. Something like this:

```cs
	class TaskControllerBuilder() 
	{
		private object[] args;
		private Guid tenantId;
		public TaskControllerBuilder WithConstructorParams(params object args ) 
		{
			this.args = args;
			return this;
		}

		public TaskControllerBuilder WithTenantId(Guid id ) 
		{
			this.tenantId = id;
			return this;
		}

		public TaskController Build() 
		{
			var mock = new Mock<TaskController>(MockBehavior.Strict, args);
			mock.Setup(t => t.TenantId).Returns(tenantId);
			return mock.Object;
		}
	}
```
### Generics 
Writing `XXXControllerBuilder` for every controller isn't even funny - that's where generics come in - so something like this might be easier:

```cs
	controller = new ControllerBuilder<EventsController>()
                .WithConstructorParams(mockOpsRepo.Object)
                .Build();
```
and the generic class as:

```cs
	class ControllerBuilder<T>() where T: Controller
	{
		private object[] args;
		private Guid tenantId;
		protected Mock<T> mockController;

		public ControllerBuilder<T> WithConstructorParams(params object args ) 
		{
			this.args = args;
			return this;
		}
		
		public T Build() 
		{
			mockController = new Mock<T>(MockBehavior.Strict, args);
			mockController.Setup(t => t.TenantId).Returns(tenantId);
			return mock.Object;
		}
	}
```
In about 2 seconds we realize that it won't work - since the constraint only specifies T should be a subclass of `Controller`, we do not have the  TenantId or SubscriptionId properties in the `Build` method.

Hmm - so a little refactoring is in order. A base `ControllerBuilder<T>` that can be used for only `plain` controllers and a sub class for controllers deriving from `TenantController`. So lets move the `tenantId` out of the way from `ControllerBuilder<T>`. 

```cs
	class TenantControllerBuilder<T>: ControllerBuilder<T>	
	 where T: TenantController 			// and this will allow access
	 									// TenantId and SubscriptionId
	{
		private Guid tenantId;
		public TenantControllerBuilder<T> WithTenantId(Guid tenantId) 
		{
			this.tenatId = tenantId;
			return this;
		}

		public T Build() 
		{
			// call the base
			var mock = base.Build();
			// do additional stuff specific to TenantController sub classes.
			mockController.Setup(t => t.TenantId).Returns(this.tenantId);
			return mock.Object;
		}
	}
```

Now, this will work as intended......Almost :

```cs
/// This will work:
controller = new TenantControllerBuilder<TaskController>()
			.WithTenantId(guid)								// Returns TenantControllerBuilder<T>
            .WithConstructorParams(mockOpsRepo.Object)		// okay!
            .Build();

///This won't compile:
controller = new TenantControllerBuilder<TaskController>()
            .WithConstructorParams(mockOpsRepo.Object) 	// returns ControllerBuilder<T>
            .WithTenantId(guid) 						// Compiler can't resolve WithTenant method.
            .Build();
```

This is basically **return type covariance** and its not supported in C# and will likely never be. With good reason too - if the  base class contract says that you'll get a `ControllerBuilder<T>`, then the derived class cannot provide a stricter contract that it will provide not only a `ControllerBuilder<T>` but that it will only be `TenantControllerBuilder<T>`.

But this does muck up our builder API's chainability - telling clients to call methods in certain arbitrary sequence is a no - no. And this is where extensions provide a neat solution. Its in two parts

* Keep only state in `TenantControllerBuilder`. 
* Use an extension class to convert from `ControllerBuilder<T>` to `TenantControllerBuilder<T>` safely with the extension api.

```cs

// Only state:
class TenantControllerBuilder<T> : ControllerBuilder<T> where T : TenantController
{
    public Guid TenantId { get; set; }

    public override T Build()
    {
        var mock = base.Build();
        this.mockController.SetupGet(t => t.TenantId).Returns(this.TenantId);
        return mock;
    }
}

// And extensions that restore chainability
static class TenantControllerBuilderExtensions
{
	public static TenantControllerBuilder<T> WithTenantId<T>(
										this ControllerBuilder<T> t,
										Guid guid)
            where T : TenantController
    {
        TenantControllerBuilder<T> c = (TenantControllerBuilder<T>)t;
        c.TenantId = guid;
        return c;
    }

     public static TenantControllerBuilder<T> WithoutTenant<T>(this ControllerBuilder<T> t)
            where T : TenantController
    {
        TenantControllerBuilder<T> c = (TenantControllerBuilder<T>)t;
        c.TenantId = Guid.Empty;
        return c;
    }
}
```

So, going back to our API:

```cs
///This now works as intended
controller = new TenantControllerBuilder<TaskController>()
            .WithConstructorParams(mockOpsRepo.Object) 	// returns ControllerBuilder<T>
            .WithTenantId(guid) 						// Resolves to the extension method
            .Build();
```

It's nice sometimes to have your cake and eat it too :D.