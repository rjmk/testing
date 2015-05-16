# Testing with Qunit for Complete Beginners

QUnit is a javascript framework that you can use to test software as you build it. Learning how to use qunit so that you can test software as you build it will save you time in the long run. Testing will ensure that you only write the least amount of code needed to get your software to do what it needs to do. It will also save you  having to constantly backtrack and fix things once your software starts growing in complexity.

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

## Step 3: Write a Test and Fail

It is important to write a test and make sure it fail before you do anything else. If you have seen a test fail then you know that the test works. When you write code to pass the test you can rest assured that the test is doing its job.

We are going to write a test to check that the header shows up on the page.

1. The first line should explain what the test does for future reference
	```javascript
	 test("check that the header exists", function() {
 	```

2. The next line sets a variable that you can then refer to in line 3. this variable should be the location of the thing you want to test.
	```javascript
	var title= document.getElementById('heading').innerHTML;
	```

3. The third line then tests the variable created in line two against what you expect it to be. The equal assetion will only pass the test if the first thing inside the brackets matches the next thing after the comma.
    ```javascript
    equal(title, "This is a header");})
    ```

When you put it all together it shoudl look like this:
```javascript
test("check that the header exist'", function() {
    var title= document.getElementById('heading').innerHTML;
    equal(title, "This is a header");
})
```

When you load the page in the browser (the page you saved the boiler plate to, not test.js) it should look like this.

![Fail test](http://i.imgur.com/Y2R8Ow9.png)

The test will fail because we have not added the header to the page. This is exactly what is suppose to happen. This is what the test explains in the red section.

##Step 4: Pass Your Test

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
