## Automated acceptance tests with Behat and Mink

An alternative to Dusk that is less Laravel-specific is Behat.

Behat doesn't include any kind of browser integration - for that you need to use Mink as well.

The combination of Behat and Mink has some advantages over Dusk, namely:

* The syntax of Behat tests is not only easier for non-developers to understand, but it makes it simpler to produce a series of steps that can be used over and over in different steps - once you have a big enough library of steps, you can test pretty much any additional behaviour easily.
* Mink allows switching drivers easily, making it straightforward to test against different web browsers. You can use browsers that don't execute Javascript for certain parts in order to get a speed boost, and then switch to another browser for the ones that need Javascript.
