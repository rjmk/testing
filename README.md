# Testing with Qunit for Complete Beginners

Qunit is a javascript framework that you can use to test software as you build it. Learning how to use qunit so that you can test software as you build it will save you time in the long run. Testing will ensure that you only write the least amount of code needed to get your software to do what it needs to do. It will also save you  having to constantly backtrack and fix things once your software starts growing in complexity.

##Step 1: Set Up the Boilerplate
To get set up go to [qunitjs.com](http://qunitjs.com/) and copy the 14 lines of boilerplate code on the homepage into a new file.

![Qunit boilerplate](http://i.imgur.com/RexzzmR.png)

##Important
Make sure to add 'http:' to the begining of the urls on line 6 and 11 otherwise it won't work. 

##Step 2: Set up the sample test
Create a new file called test.js (if you call it something different you have to change the refernce to it on line 12 of the boilerplate).

Inside test.js copy and paste the sample test from the Qunit homepage.

![Sample Qunit Test](http://i.imgur.com/vSdTBOf.png)

To keep things simple remove everything inessential to the test so that it just looks like:

```javascript
test( "this bit should describe, to a human, what the test is for", function() {
assert.ok( 1 == "1");
});
```

Then change:
```javascript
assert.ok( 1 == "1", "Passed!" );
```

to:
```javascript
equal( 1, "1");
```

'equal' and 'assert.ok' are two different types of tests (in Qunit tests are called assertions). 'equal' and 'asssert.ok' pretty much do the same thing. I recommed using the 'equal' assertion instead of 'assert.ok' to keep things simple for now. Here are [a list of all the different kinds of assertions](http://api.qunitjs.com/category/assert/) you can use once you get the hang of how to write tests.

## Step 3: Write a Test and Fail

It is important to write a test and make sure it fail before you do anything else. If you have seen a test fail then you know that the test works. When you write code to pass the test you can rest assured that the test is doing its job.

We are going to write a test to check that the header shows up on the page.

1. The first line should explain what the test does for future reference
	```javascript
	 test("check that the header exist'", function() {
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

> <h1 id='heading'>This is a header</h1>

A passing test will have a nice green line along it like this:

![Pass test](http://i.imgur.com/je0769P.png)

Failing a test before you wrote the code needed to pass it is the whole idea behind test driven development. Working in this way ensures that 
you write the least amount of code needed to get your software to do what it needs to do. 

Ideally testing fits in after you have all your user stories. User stories help you understand the tests you will need and the test will drive the code.


If you want more practice this is [the tutorial we used](https://github.com/docdis/learn-qunit);


