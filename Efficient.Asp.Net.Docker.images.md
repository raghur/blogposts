<!--
PostId: 8793131475245183754
Title    : Building slim .NET core docker images
Labels   : docker, .netcore, tips, HOWTO
Format	 : markdown
Published: true
-->

As I play with .NET core, I was getting a little frustrated with waiting for `docker build` and `docker push` to complete - the docker images for
.net core are pretty large - weighing in at a gig as shown below. My trivial web service is hardly 80mb with dependencies - so this did not feel good at all.

```sh
rraghur/webapi-app                                    v3                  9ff77929c379        25 hours ago        976.5 MB
```

The .net project template sets up a `Dockerfile` - and while this is great,
it's geared more towards development rather than production. It goes something like this:

```docker
FROM microsoft/dotnet:latest

COPY . /app
WORKDIR /app
RUN ["dotnet", "restore"]
RUN ["dotnet", "build"]
EXPOSE 5000/tcp
CMD ["dotnet", "run", "--server.urls", "http://*:5000"]

```

Here's the trouble - it basically needs the sdk and builds and runs the app inside the container. Also problematic is that the source is pushed inside the container. What if we just push binaries and use only the runtime?

First, make the app self-contained (SCD). In `project.json`, you need to remove the platform dependency type (since the platform's going to be packaged with the app). Then also add `ubuntu.14.04-x64` under `runtimes` -

```json
 "runtimes": {
    "win81-x64": {},
    "ubuntu.14.04-x64": {},
    "debian.8-x64": {}
  },
```

Second, let's publish - `dotnet publish -r ubuntu.14.04-x64`. This will generate the app with all dependencies in `bin/debug/netcoreapp1.1/ubuntu-14.04-x64/publish`

Finally, let's update the docker file to package this in:

```docker
FROM microsoft/dotnet:1.1-runtime-deps

COPY ./bin/Debug/netcoreapp1.1/ubuntu.14.04-x64/publish /app

WORKDIR /app

EXPOSE 5000/tcp

CMD ["./WebAPIApplication", "--server.urls", "http://*:5000"]
```

With the above, a docker build results in a much more svelte 230MB image - that's a 76% reduction in fat :)!

```sh
rraghur/webapi-app                                    v5                  815cbcecaea9        19 seconds ago      230.8 MB
```

Let's take it out for a spin:

```sh
D:\WebAPIApplication>docker run -it --rm rraghur/webapi-app:v5
Hosting environment: Production
Content root path: /app
Now listening on: http://*:5000
Application started. Press Ctrl+C to shut down.

```

Keep shipping!
