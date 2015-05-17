# Testing with QUnit for Complete Beginners

QUnit is a javascript framework that you can use to test software as you build it. Learning how to use QUnit so that you can test software as you build it will save you time in the long run. Testing will ensure that you only write the least amount of code needed to get your software to do what it needs to do. It will also save you  having to constantly backtrack and fix things once your software starts growing in complexity.

## How to use this README

Testing turns out to be quite complicated! There's a lot of information that's useful to know. Unfortunately, trying to read all that information at once is not a good way to learn it. This README has some important information that should be ignored if it's not immediately useful and / or it is your first time reading it. Currently, this includes the 'Under The Hood' sections, which talk a little about how QUnit works on a code level. It's quite useful as it will reduce your confusion as you continue to work with QUnit, but is safely ignored on a first readthrough. Get used to mucking around with QUnit a bit first.

## 0: What are testing and TDD and why should I care?

Testing frameworks allow you to check the functionality of every part of your website automatically, without going through everything yourself. As your website and its functionality grows, the number of different ways it can be used grows rapidly. If a user does everything on your website once and your website has three things to do, there are six different sequences in which you can do them. If you have five things to do, there are 120 different sequences! Even if you can't use the functionality on your website in a completely arbitrary order, it will quickly become impractical to check every user flow remains functional after every update. Writing a suite of tests allows us to specify the tests once, and run them as often as we like.

This might seem like reason enough to write tests, but it doesn't explain why you would want to use Test-Driven Development (TDD) and write the tests *first*. This is where the real power comes, however. Writing tests is essentially writing *actionable specifications*. Say you want a user to be able to navigate your site easily. When we write tests, we have to boil this down to specific, concrete features. Is their any font smaller than a certain acceptable size? Can I reach every page in some number of clicks? Once we have tests that specify our requirements in a solveable manner, all that's left is to fulfil them.

Two comments are worth making here. The first is to introduce a useful distinction, that between *acceptance* and *unit* tests. An *acceptance* test is high-level, and connects directly with our user stories. This might be quite granular, like seeing a blank timer when loading a stopwatch app, or more of a flow, like the ability to start and pause the stopwatch with visible buttons. A *unit* test is testing a small piece of functionality. It is built much more with the developer than the user in mind and will be helpful for debugging. You might have a unit test for a *function* in your code, and a separate one to check its invocation when some element of the UI is interacted with. Knowing which part is breaking will be helpful for maintaining your site.

The second comment is on the adage 'what gets measured gets done'. Some of you might have been concerned earlier when the goal of 'navigating the site easily' was transformed into a finite list of concrete tests. It's true that we can't *really* directly test that goal. In fact, the only way to do that is to have real humans interact with the site. But our tests are much cheaper and can be deployed whenever we like. If you or a user identifies a problem that is making your site less navigable, work out how it could be improved, add a test, and fix it! No amount of tests will ever cover *all* edge cases, but as we add more tests, so our coverage will grow.

##Step 1: Set Up the Boilerplate
Let's set up with some boilerplate from the [qunit website](www.qunitjs.com). Create a new directory for your testing adventures, and create a file called (for example) `tests.html` with the following content:

```
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>QUnit Example</title>
  <link rel="stylesheet" href="https://code.jquery.com/qunit/qunit-1.18.0.css">
</head>
<body>
  <div id="qunit"></div>
  <div id="qunit-fixture"></div>
  <script src="https://code.jquery.com/qunit/qunit-1.18.0.js"></script>
  <script src="tests.js"></script>
</body>
</html>
```

##Step 2: Set up the sample test
Create a new file called `tests.js` (if you call it something different you will have to change the reference to it on line 12 of the boilerplate).

Inside `test.js` copy and paste the following code:

```javascript
test("this bit should describe, to a human, what the test is for", function() {
	equal( 1, 1);
});
```

Equal checks that two objects are equal to each other. If you save this and load your tests.html file in a browser, you should see a basic passing test. Something that will be very important for our testing is whether equal uses `==` (with type coercion), or `===` (without type coercion). Let's check which is the case by editing our `tests.js` file.

Edit the second line of your test to read
```javascript
equal('1', 1)
```

Will this be true if `equal` uses `==`? What about `===`? Reload your page and check to see whether it passes or not.

If you want to use the other sort of equality, you can use a different function called `ok`, which will make the test function it's inside pass if its first parameter is true. You can then use the equality you want by hand inside that parameter, like this:

```javascript
test('this is a strict test', function(){
	ok('1'===1);
});
```

`equal` and `ok` are known as assertions. QUnit provides a variety of these functions to help with testing. If every assertion inside a test function call returns true, so does the overall. Here is [a list of all the different assertions QUnit provides](http://api.qunitjs.com/category/assert/). Try putting one passing and one failing assertion in your test and see QUnit's output.

### Under The Hood: Side Effects & Return Values

Notice that the callback function (the second parameter provided to test) has no `return` keyword. That means that it will return `undefined`, no matter what the assertions (`ok`, `equal`, etc.) do. So clearly `test` isn't doing anything with the return value of the callback function! How do the assertions provide the test function with the information it needs then?

Every time test is called, it creates a new object where the results of assertions are kept. Assertions then push their results (which normally include boolean values, the expected result and the actual result) to this object. The test then checks this object to see if everything is OK. Assertions causing changes to the outside world (this object in the scope of the test function) is known as them having 'side effects'. Subjects which have no side effects are known as 'pure functions'. They can still be useful as long as they return an interesting result.

It's worth noting that this means that assert cannot be called outside of a test function: it will be looking for an object that doesn't exist!

## Step 3: Write a Test and Fail

As mentioned above, you should write your test *first* and make it pass *second*. There are several good reasons for this. The first and most important is for creating specifications as mentioned above. It also means that your test is useful! If you write a test that should only pass once you implement some new functionality, you want to see it fail! If it doesn't, you aren't testing anything, and even broken code will pass.

We are going to write a test to check that the header shows up on the page.

1. The first line should explain what the test does for future reference
	```javascript
	 test("check that the header exists", function() {
 	```

2. The next line sets a variable that you can then refer to in line 3. We are looking for an element with the ID 'heading', and storing its text in the variable.
	```javascript
	var title = document.getElementById('heading').innerHTML;
	```

3. The third line then tests the variable created in line two against what you expect it to be. The equal assertion will only pass if the first parameter matches the second.
    ```javascript
    equal(title, "This is a header");})
    ```

When you put it all together it should look like this:
```javascript
test("check that the header exist'", function() {
    var title= document.getElementById('heading').innerHTML;
    equal(title, "This is a header");
})
```

When you load the page in the browser (the page you saved the boiler plate to, not test.js) it should look like this.

![Fail test](http://i.imgur.com/Y2R8Ow9.png)

The test will fail because we have not added the header to the page. This is exactly what is suppose to happen. This is what the test explains in the red section.

### Fictional Q & A
Q: But this test only checks that there is an item with a certain ID that has a certain text. What if I put that ID a span tag? What if I decide I want a different title?

A: This goes back to section 0. You're right our test could break in all those ways. For now we're making some reasonable assumptions. By adding more tests and tweaking the ones we have, we can make our application much stronger and catch more possible failure points. For now, let's keep it simple so we don't have to deal with traversing the DOM too much (not the easiest thing without jQuery).

## Step 4: Pass Your Test

Add a h1 tag with an id of 'header' that says "This is a header" to the page and rerun the test.

```html
<h1 id='heading'>This is a header</h1>
```

A passing test will have a nice green line along it like this:

![Pass test](http://i.imgur.com/je0769P.png)

Failing a test before you wrote the code needed to pass it is the whole idea behind test driven development. Working in this way ensures that
you write the least amount of code needed to get your software to do what it needs to do.

Ideally testing fits in after you have all your user stories. User stories help you understand the tests you will need and the test will drive the code.


If you want more practice this is [the tutorial we used](https://github.com/docdis/learn-qunit);
