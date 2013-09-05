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
2. Good unit tests
3. Mocking in python

#### Packaging
4. Packaging with `setup.py`
7. `pip`, `setuptools`, `easy_install` and their idiosyncracies
5. Install a platform specific script
5. PyPI - registering and publishing

#### Testing
6. `virtualenv`
7. Testing platform specific scripts installed with tools above
8. Coverage

#### Coding/Style/Syntax linting
8. `pyflakes`, `pylame`, `pep8` and integration in Vim with Syntastic

### What I really, really liked
2. That I didn't miss a debugger
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
