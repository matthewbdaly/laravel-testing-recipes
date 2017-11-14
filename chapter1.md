# Why write tests?

Whenever the subject of test-driven development comes up, my experience has been that a lot of developers, managers and other stakeholders are resistant to the idea. You may even be one of them! Well, I'm here to tell you that learning to write tests is the single best thing I've done as a professional web developer. The benefits of having a full test suite include:

* Easier debugging
* Easier upgrades
* Better code
* Easier to understand

And that's just for starters! However, you're still likely to hear a lot of arguments against writing tests. I've collected these below, together with refutations:

> Can't you just check you get it right?

This will only work for the most trivial of sites. Otherwise, you're stuck with manually checking the site behaves as expected every time you make a change. Do you relish that prospect? I certainly don't - I'm too lazy and my time is too expensive to my employer for them to want to do that.

Software development is one of the hardest things humans have done. The best developers in the world can't guarantee all of their code is 100% perfect and bug free - even NASA, with their gargantuan budget, have gone wrong at times. It's entirely unreasonable to expect every developer to get it right 100% of the time or catch every error they may have introduced. So taking steps to actively watch for any bugs is prudent.

The best analogy I've ever heard is with double-entry bookkeeping in accountancy. Accountants need to be able to double-check that their records are correct, so they enter them twice and make sure the results match up. Tests do the same thing in that they express the logic of an application in a separate place, to help ensure that the implementation and intent match up. Accountancy isn't as demanding as programming, yet we seem to think we're infallible. Truth is, we aren't, and never will be, so we need all the help we can get.

> We don't have time to write tests

And you do have time to fix bugs in production that could have been found earlier? The earlier a bug is found, the lower the cost to fix it. Once code is in production, the potential cost of fixing problems with it becomes much higher than if it were detected straight after implementation, because the code I've written is still fresh in my mind.

I've worked on projects with and without tests. When working on the ones that had tests, I often found problems immediately after I'd implemented the solution, making it easy to fix them. When I didn't have tests, it was often weeks or months later that I found the issue, potentially causing problems with a project. Upgrades to software packages have been quicker, easier and less stressful because I've been able to identify issues during the upgrade process. And it was quicker to reproduce the issue with tests because I could set a breakpoint at an appropriate point in the existing test suite and examine what's happening directly.

There's no denying that writing tests does add time to the software development process. However, you do get a return on that investment of time. A project with good test coverage is easier to work on, to upgrade and extend, and to maintain. By catching errors as early as possible, a good test suite will save you the grief of explaining to your manager why your shiny new web app failed in production, rather than allowing you to catch that error in development.

Also, manual testing is not only time-consuming, but incredibly dull. Computers can repeat the same set of steps over and over easily without human intervention, so let them play to their strengths by automating the tests you'd otherwise do manually.

> You can't guarantee every error will be caught

No, you can't. Writing tests is not a silver bullet. There are often bugs that slip through the test suite and make it into production, and that can't be helped. However, once you're aware of a bug, you can write a test to catch it, and then fix it, so as to make sure that error will never appear again.

> How can you guarantee these tests are of any use?

Browser-based acceptance tests are the easiest to justify to non-technical users. They visibly work with the application by driving a real web browser to interact with it to ensure that it works as specified. They're also often the easiest way to get started writing tests. However, they have the disadvantage of being very much slower than unit tests, and are therefore not usually run as part of the standard test suite.

Lower-level integration or unit tests are harder to justify. Someone else may not be able to see how your application is being tested, and therefore not be certain whether it's actually testing it or not. They may also feel more abstract, especially if you're mocking out some of the dependencies in your tests. For this, it's a good idea to generate code coverage stats regularly to make sure that you can demonstrate what proportion of your application is tested.

OK, you've convinced me. What do I need to know?
------------------------------------------------

You'll hear a lot of different types of tests mentioned, such as end-to-end tests, functional tests, and so on. In general, there are three main types of tests you should be aware of:

*Acceptance tests* are written from the perspective of an end user, and dictate **what an application must do to be acceptable**. This may mean writing the test in a manner that can be understood by business stakeholders or other non-technical team members. Typically this might be summed up as something like this:

> When I click the "Post" button, my latest blog post should be posted

*Integration tests* are lower level tests that interact with more than one unit of code within your application. For instance, if you had a REST API that retrieved recipes, you might test that a request searching for *Banoffee Pie* returned a result from the `recipes` endpoint.

*Unit tests* work on a single "unit" of code, such as a class or function, and any dependencies of that unit must be replaced by mocks. For instance, if you wrote a test for an ORM model that in turn called the database, that would not technically be a unit test - for it to meet the definition of a unit test, the database connection would need to have been mocked. 

What do I need?
---------------

An existing Laravel project will already have the required third-party packages available, but if you're building a standalone package, you'll need to include them yourself. They are:

* PHPUnit
* Mockery
* PsySh

I'll explain all of these at a more appropriate time. In addition, you may want to use Behat and Mink for acceptance testing - we'll cover those in the first part as they are a good introduction to writing tests, but they're also arguably less essential than the others.
