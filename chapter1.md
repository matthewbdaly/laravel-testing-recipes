# Why write tests?

Whenever the subject of test-driven development comes up, my experience has been that a lot of developer, managers and other people are resistant to the idea.

> Can't you just check you get it right?

Software development is one of the hardest things humans have done.

> We don't have time to write tests

And you do have time to fix bugs in production that could have been found earlier? The earlier a bug is found, the lower the cost to fix it. Once code is in production, the potential cost of fixing problems with it becomes much higher than if it were detected straight after implementation, because the code I've written is still fresh in my mind.

I've worked on projects with and without tests. When working on the ones that had tests, I often found problems immediately after I'd implemented the solution, making it easy to fix them. When I didn't have tests, it was often weeks or months later that I found the issue, potentially causing problems with a project.
