<!--
PostId: 595575659130069894
Title    : What publishing a python module taught me
Labels   : python,programming, blogger
Format	 : markdown
-->
### Programming in Python
So I've mostly used python for one off scripts and tools and at one point for a serious foray into Django - but never
came into a situation where I'd thought of publishing anything.

Hmm - crossed that bridge over this weekend - and its been a fun journey. I'm writing this post with what I wrote :)

**Things I've picked up**

#### Code

1. Better understanding into Python modules, classes and code organization for libraries.
    * [Be Pythonic: __init__.py](http://mikegrouchy.com/blog/2012/05/be-pythonic-__init__py.html)
2. Good unit tests
3. Mocking in python
    * [Python Mocks](http://www.voidspace.org.uk/python/mock/)
    * [Mocking patterns: chained calls, partial mocking and open as context manager](http://www.voidspace.org.uk/python/weblog/arch_d7_2010_10_02.shtml)


#### Packaging

1. Packaging with `setup.py`
    * [What I used to get started](http://pythonhosted.org/an_example_pypi_project/setuptools.html)
    * [Reference](http://pythonhosted.org/distribute/setuptools.html)
7. `pip`, `setuptools`, `easy_install` and their idiosyncracies
5. Install a platform specific script
    * [reference](http://pythonhosted.org/distribute/setuptools.html#automatic-script-creation)
5. PyPI - registering and publishing
    * [Tutorial](http://docs.python.org/3/distutils/packageindex.html)
        One point to note - if you create the `.pypirc` manually,  you will need to manually do a 
        `python setup.py register`. If you skip that, `python setup.py sdist upload` will fail with 
        a 403.

#### Testing

1. `virtualenv` - [this link](http://simononsoftware.com/virtualenv-tutorial/)
7. Testing platform specific scripts installed with tools above
8. Coverage

#### Coding/Style/Syntax linting

1. `pyflakes`, `pylame`, `pep8` and integration in Vim with Syntastic

### What I really, really liked

1. That I didn't miss a debugger
3. That tests were short and sweet
4. good code coverage out of the box
5. `pip`
1. Overall, how pleasant it was and how much I enjoyed it. 

### Where I had hiccups

1. Mocks in python were a little hard to debug/understand
2. Should have written the tests first - but it came as an afterthought after I decided to publish.
3. Finding good documentation on packaging - for ex:, its hard to find a good walkthrough of how to publish

Just putting finishing touches and a little polish for a v0.9 release. Basically this post itself is nothing
but a test. Should be out with it in a day or two.
