---
layout: post
title:  "The Prime Factors Kata"
date:   2020-01-13 20:33:48 +0000
categories: jekyll update
---
In the world of software development, test-driven development (TDD) is a discipline where failing tests are written first, and *only then* is the actual software code created, aiming to pass the newly-generated tests.

In this article, I will explore the fundamental components of test-driven development, with a step by step guide on how to incrementally develop an algorithm to get all the [prime factors](https://en.wikipedia.org/wiki/Table_of_prime_factors) of a given integer, using TTD.

So without further ado, let's get started!

## What is Test-Driven Development?

The core of the test-driven development cycle revolves around five simple steps, which are repeated throughout the software development life cycle. The five steps are:

1. Write a test
2. Run all Tests and Confirm the Test Fails
3. Write Code to Pass Test
4. Run the Tests and Confirm the Test Passes
5. Refactor

### 1. Write a Test

In test-driven development, each new cycle begins with writing a test. This test should be as succint and as simple as possible, testing a very specific aspect or component of a larger feature. This is the first differentiating feature of TTD versus writing unit tests *after* the code is written, as it makes the developer focus on the requirements *before* writing the code.

We can now start with the implementation of the prime factors algorithm, by writing the first test. We will use the Ruby language and the RSpec framework for testing.

Let's create a new project folder and initialize a new RSpec environment:

```shell
mkdir prime_factors_kata
cd prime_factors_kata
rspec --init
```

Now, we are ready to create our test file inside the ```spec``` folder:

```shell
touch spec/prime_factors_spec.rb
```

Let's now write the first test, which should aim to be the simpliest possible. In this case we expect the integer ```1``` to not have any prime factors.

```ruby
# in prime_factors_spec.rb
describe "prime_factors" do
  it "should return an empty array if given 1" do
    expect(prime_factors(1)).to eq []
  end
end
```

### 2. Run all Tests and Confirm the Test Fails

Once the test is created, the next step is to confirm that the test fails. By confirming that the new test fails and does so for the reasons you expect, you can be confident that the test is useful, and will be beneficial once you write the code necessary to pass the test.

Let's run the tests with ```rspec```.

```shell
NameError:
  undefined local variable or method `prime_factors' for main:Object
# ./spec/prime_factors_spec.rb:1:in `<top (required)>'
```
### 3. Write Code to Pass Test

After confirming that the test fails, you now must write the code that will allow the test to pass. The idea here is that you *should not* write any additional code that goes beyond the scope of the test, but only focusing on writing the least amount of code to make the test pass.

The code written here will likely be rough and not finalized, but that's ok as the entire test-driven development process encourages constant refactoring, meaning your current code is likely to change numerous times in the future.

We can now start writing our production code, letting the error messages guide us. Let's create the production code file ```prime_factors.rb``` inside the ```lib``` folder.

```shell
mkdir lib
touch lib/prime_factors.rb
```

Let's now define the method ```prime_factors``` and require the production code file inside the test file.

```ruby
# in prime_factors.rb
def prime_factors
end
```

```ruby
# in prime_factors_spec.rb
require "prime_factors"
```

Now it's time to run the tests again, with ```rspec```.

```shell
ArgumentError:
  wrong number of arguments (given 1, expected 0)
# ./lib/prime_factors.rb:1:in `prime_factors'
# ./spec/prime_factors_spec.rb:4:in `block in <top (required)>'
# ./spec/prime_factors_spec.rb:3:in `<top (required)>'
No examples found.
```

The error message has changed and this is good news, as it means the code we have just written has been useful to solve the previous error message.

If we follow the new error message, we can now add an argument to the method ```prime_factors```.

```ruby
# in prime_factors.rb
def prime_factors(number)
end
```

Let's run the tests again.

```shell
Failure/Error: expect(prime_factors(1)).to eq []
     
  expected: []
    got: nil
     
(compared using ==)
# ./spec/prime_factors_spec.rb:5:in `block (2 levels) in <top (required)>'
```

The error message has changed again, which is good. In order to make the test pass with the least amount of code, we can just return an empty array.

```ruby
# in prime_factors.rb
def prime_factors(number)
  []
end
```

### 4. Run the Tests and Confirm the Test Passes

Once your new code is written, confirm that the test now passes. Let's run ```rspec``` again.

```shell
.

Finished in 0.00259 seconds (files took 0.0979 seconds to load)
1 example, 0 failures
```

### 5. Refactor

The process of writing the code necessary to allow the test to pass may have introduced some duplications or inconsistencies. 



## Incremental Algorithm Implementation






Over the years, test-driven development fine granularity has been codified into three rules: the so-called *three laws of TTD*.

1. You must write a failing test before you write any production code.
2. You must not write more of a test than is sufficient to fail, or fail to compile.
3. You must not write more production code than is sufficient to make the currently failing test pass.








The three laws are the *nano-cycle* of TDD and you realize that you simply cannot write very much code at all without compiling and executing something. The point is that whether we are writing tests, writing production code or refactoring, we keep the system executing at all times.

## The Red-Green-Refactor Cycle ##

The red, green, refactor approach helps developers compartmentalize their focus into three phases:

- Red: think about *what* you want to develop.
- Green: think about *how* to make your tests pass.
- Refactor: think about *how* to improve your existing implementation.



This cycle is typically executed once for every complete unit test. The rules of this cycle are simple.

1. Create a unit test that fails.
2. Write production code that makes that test pass.
3. Refactor the code.


You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
