# Jenkins build flows

I've been busy setting up a jenkins server for our ci and deployment duties. We were using the free version of Teamcity
but hitting up against the 20 config limit.

Jenkins is a great Ci solution with an awesome community. It's greatest redeeming feature and simultaneously the most
exasperating flaw is the huge plugin eco system.

Chances are that just jenkins on its own is not going to cut it for you if your CI requirements are anything beyond basic.
The good thing is that there's probably a plugin out there that'll let you do what you need to do with jenkins. Problem is that
it takes a lot of research, tweaking and configuration to get things working the way you want them to. Also, with plugins,
it's easy to find out that different plugins don't work with each other well or they need 'special' treatment. That said,
I don't think there's another open CI system that I'd rather use.

That said, let me put this blog post down for the future me who comes here needing to set up Jenkins just right...

## Goals

1. DRY - as much as possible do not repeat build configurations. We have multiple projects that all do the same thing:
    1. a debug build config that's triggered on checkin that runs unit tests
    2. a packaging build that packages projects into nugets and pushes it to nuget repos.
    3. a dev deploy - debug build + package + deploy
    4. a prod deploy - ditto - but release build and to prod
Ideally, I'd like to just be able to set up base builds that can be 'inherited'/'composed' for each project. Updating the base build should be sufficient.
2. Easy to spin up new build configs - goes with 1 - but should be easy to set up a new build config.
3. Flow/orchestration:
    - should be possible to invoke downstream builds that use upstream build areas/outputs.
    If you notice above, the dev deploy is really going to run the debug ci and then package - why repeat that configuration?


