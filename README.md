# Laravel Testing Recipes

Laravel is a pretty popular choice of framework right now. It's largely supplanted CodeIgniter as the easiest framework to get started with in PHP, but at the same time it's flexible enough that in can grow with you.

One notable feature of Laravel compared to earlier frameworks such as CodeIgniter is the greater emphasis it places on testing. It comes with both PHPUnit and Mockery installed by default, and the documentation strongly encourages you to write tests for your application. However, that doesn't necessarily mean that it's obvious how to test every component of your application.

That's where this book comes in. Over the last couple of years I've used Laravel extensively and I've formulated strategies for writing tests for most of the components of a typical Laravel application. I won't pretend this book is exhaustive, but it will give you recipes for testing the major components of your Laravel application, and if you understand it reasonably well, you should be able to apply the same strategies to other areas of your application.

About me
--------

To this day, I don't really consider myself first and foremost a PHP developer. I came to web development quite late in life - I was previously a clerical worker before I started to make plans for a new career in my late 20's. I always planned to work first and foremost with Python, specifically the Django framework, but things didn't quite work out that way as PHP is much more prominent where I live. My first web development job was doing PHP development for an e-commerce site, and there I started using CodeIgniter. I later moved on to work at a busy digital agency doing web and Phonegap app development, and I initially stuck with CodeIgniter, although I later started using Django as well.

In 2015 we had another developer join and it became obvious that we needed to make a decision regarding what framework we used by default. I couldn't justify continuing to use Django because none of the other developers knew Python, but at the same time CodeIgniter wasn't up to scratch - its future was uncertain and it had fallen behind the times. I'd been following Laravel with interest for some time, and was already using it on a project. In addition, the developer we'd just hired had also used it, so it was a logical choice.

Since then, there have been many projects I've worked on using Laravel. After moving to a new role, I've taken on a large Laravel legacy project and added tests to it retrospectively. I've tested API's, front ends, and just about everything in between, and I've learned a tremendous amount in the process. Now, hopefully I can help you avoid some of the problems I experienced during that time.
