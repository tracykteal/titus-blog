Testing is fun
##############

:author: C\. Titus Brown
:tags: python,testing,figleaf
:date: 2007-10-29
:slug: testing-is-fun
:category: python


I spent a fairly satisfying two days setting up unit and functional
tests for figleaf, my code coverage package.  I implemented the tests
using a technique that I gather is called `vertical slicing
<http://www.think-box.co.uk/blog/2007/10/vertical-slicing.html>`__ --
I just called it "starting a new phase of a project" before ;).  The
idea is that you implement a single test, with necessary fixtures,
from end to end; then you widen it.  This way you don't get overwhelmed
with planning ahead for different features or other distractions: you
just get one thing working, with the attendant psychological reward
of satisfaction.

Why all the testing?  Well, after twill, figleaf is the most popular
of my packages, but it is also much uglier than it should be.  A number
of bug reports have been sent to me, and I need to deal with them.  Since I
am also hoping to shoehorn it into a large Python-based project, this is
an apropos time.

Some of the fun new features I am addign for figleaf 0.7 include a
much enhanced and more configurable reporting/annotating interface,
and (something I'm particularly proud of) tracing of statements only
within specific files.

Anyway, I ran into a few entertaining problems with the tests.

First, python2.4 and python2.5 differ in what they consider an executed
statement.  Most notably, python2.4 will call the trace function when
'pass' is executed, while python2.5 will not.  This means my regression
tests need to compare against version-specific "known good" data.

Second, easy_install was mucking around with my imported packages and
I couldn't get the figleaf-under-test to be imported ahead of the
installed version.  It turns out that this code re-scans sys.path and
thus imports the correct package: ::

   import figleaf  # probably unnecessary
   sys.path.insert(0, '/path/to/correct/figleaf')
   reload(sys.modules['figleaf'])

Third, testing command-line executable programs is a huge pain in the
butt.  There's room for someone to come up with a framework for this
purpose.  Right now I think texttest is probably the best framework,
but I have to say that it seems overcomplicated for what it does; it
would be nice to have something simpler.  I think that the main
features that a test framework for command-line programs needs are a
way of compactly comparing test output to known-good output, and a
nice way of specifying configurations (directory locations, config
files, etc) so that a variety of features can be tested.

Fourth and finally, achieving true test isolation is a necessary art.
Because I was fiddling with sys.settrace and some
module-global-singletons in the code under test, I had to be very
careful to include appropriate tests in the setup code and appropriate
teardown code to remove/reset everything.  I kept on running into new
areas where this was a problem, but it taught me a lot about where my
own code kept state.  Very useful.

cheers,
--titus


----

**Legacy Comments**


Posted by Noah Gift on 2007-10-29 at 07:52. 

::

   Cool stuff!  I am interested to see where you go with figleaf.  Code
   coverage testing is a great thing to automate, keep up the good work.


Posted by Darius Bacon on 2007-11-13 at 17:14. 

::

   Hi Titus -- for testing command-line programs you might enjoy or at
   least be amused by my  <a href="http://www.accesscom.com/~darius/tmp/t
   ush.tgz">http://www.accesscom.com/~darius/tmp/tush.tgz</a>    It's
   probably too simple, but that makes a nicer start than too complicated
   does.

