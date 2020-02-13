---
layout: post
title:  "JS-How to Incrementally Develop an Algorithm using Test Driven Development - The Prime Factors Kata"
date:   2020-01-13 20:33:48 +0000
categories: jekyll update
---
In the world of software development, test-driven development (TDD) is a discipline where failing tests are written first, and *only then* is the actual software code created, aiming to pass the newly-generated tests.

In this article, I will explore the fundamental components of test-driven development, with a step by step guide on how to incrementally develop an algorithm to find all the [prime factors](https://en.wikipedia.org/wiki/Table_of_prime_factors) of a given integer, using TTD.

## What is Test-Driven Development?

The core of the test-driven development cycle revolves around five simple steps, which are repeated throughout the software development life cycle. The five steps are:

1. Write a new test
2. Run all tests and confirm the new test fails
3. Write code to make the new test pass
4. Run the tests and confirm all tests pass
5. Refactor the code

### 1. Write a New Test

In test-driven development, each new cycle begins with writing a test. This test should be as simple as possible, testing a very specific component of a larger feature. This is the first differentiating feature of TTD versus writing unit tests *after* the code is written, as it makes the developer focus on the requirements *before* writing the code.

Having described the first step of the TTD cycle, we can now start implementing the prime factors algorithm, by writing the first test. We will use JavaScript with the Jasmine testing framework.

```javascript
describe("primeFactors", function() {
  it("should return [] if given 1", function() {
    expect(primeFactors(1)).toEqual([])
  })
})
```

### 2. Run All Tests and Confirm the New Test Fails

Once the test is created, the next step is to confirm that the test fails. By confirming that the new test fails and does so for the reasons we expect, we can be confident that the test is *useful*, and will be beneficial once we write the code necessary to pass the test.

Let's run the tests:

```shell
ReferenceError: primeFactors is not defined
```

### 3. Write Code to Make the New Test Pass

After confirming that the test fails, we now must write the code that will allow the test to pass. The idea here is that we *should not* write any additional code that goes beyond the scope of the test, but only focusing on writing the *least amount of code* to make the test pass.

The code written here will likely be rough and not finalized, but that's ok as the entire test-driven development process encourages constant refactoring, meaning our current code is likely to change numerous times in the future.

We can now start writing our production code, letting the error messages guide us.

```javascript
function primeFactors() {}
```

Now it's time to run the tests again:

```shell
Expected undefined to equal [  ].
```

The error message has changed and this is a good news, as the code we have just written has solved the previous error message.

In order to make the test pass with the least amount of code, we can just return an empty array:

```javascript
function primeFactors() {
  return []
}
```

### 4. Run the Tests and Confirm all Tests Pass

Once our new code is written, we need to confirm that the test now passes:

```shell
1 spec, 0 failures
```

### 5. Refactor the Code

The process of writing the code necessary to allow the test to pass may have introduced some duplications or inconsistencies. That's perfectly normal and expected and it depends on the fact that *it is hard to write good code when we try to make it work*.

The importance of the refactoring step is to *take the time* to locate those problem areas and to focus on simplifying the codebase, confirming that we haven't accidentally introduced any additional bugs, or changed something that now causes a previously passing test to fail.



## Incremental Algorithm Implementation

Now that we have analysed all five steps of test-driven development, we are ready to continue implementing the algorithm and learn how to incrementally develop the code test by test.

### Prime Factors of 2

Let's write the second test. The prime factors of ```2``` is an array with a ```2``` in it:

```ruby
# in prime_factors_spec.rb
it "should return [2] if given 2" do
  expect(prime_factors(2)).to eq [2]
end
```

If we run the tests again we expect the second test to fail:

```shell
. F

Failures:

  1) prime_factors should return [2] if given 2
     Failure/Error: expect(prime_factors(2)).to eq [2]
     
       expected: [2]
            got: []
```

We can make this pass by modifying our algorithm. Although we still want to return an array, we want to be able to modify it before returning it. So, we need to create a variable called ```factors``` and introduce a ```if``` statement.

```ruby
# in prime_factors.rb
def prime_factors(n)
  factors = []
  if n > 1
    factors.push(2)
  end
  factors
end
```

If we run the tests again we expect them to pass:

```shell
. .

Finished in 0.00265 seconds (files took 0.10833 seconds to load)
2 examples, 0 failures
```

It may look like to some of you that we have just hacked an ```if``` statement and that there was no design in there. However, watch what happens to that ```if``` statement, which is fascinating.

### Prime Factors of 3

Next test. The prime factors of ```3``` is an array with the ```3``` in it:

```ruby
# in prime_factors_spec.rb
it "should return [3] if given 3" do
  expect(prime_factors(3)).to eq [3]
end
```

The third test should fail. Notice we are getting into a *hypothesis experiment loop* - we write a test and we expect it to fail:

```shell
. . F

Failures:

  1) prime_factors should return [3] if given 3
     Failure/Error: expect(prime_factors(3)).to eq [3]
     
       expected: [3]
            got: [2]
```

Our hypothesis was correct. Now, we need to make this test pass with the *fewest keystrokes possible*. Inside the ```if``` statement we can change the ```2``` to ```n``` and now the tests will pass:

```ruby
# in prime_factors.rb
def prime_factors(n)
  factors = []
  if n > 1
    factors.push(n)
  end
  factors
end
```

```shell
. . .

Finished in 0.0046 seconds (files took 0.09774 seconds to load)
3 examples, 0 failures
```

Note that was a change from a constant to a variable - a change from something *specific* to something more *general*. In fact, if you think carefully now, all the changes we have made to the production code have been towards generality. We didn't have to do that - we could have put more ```if``` statements in. However, that would violate this new rule - *as the tests get more specific, the code gets more generic*.

### Prime Factors of 4

Next test. The factors of ```4``` is an array with two ```2``` in it:

```ruby
# in prime_factors_spec.rb
it "should return [2, 2] if given 4" do
  expect(prime_factors(4)).to eq [2, 2]
end
```

We expect it to fail:

```shell
. . . F

Failures:

  1) prime_factors should return [2, 2] if given 4
     Failure/Error: expect(prime_factors(4)).to eq [2, 2]
     
       expected: [2, 2]
            got: [4]
```

If we look at the code, we can get it to pass by putting another ```if``` statement that checks whether the number is divisible by ```2``` and then reduces the original number by the factor ```2```:

```ruby
# in prime_factors.rb
def prime_factors(n)
  factors = []
  if n > 1
    if n % 2 == 0
      factors.push(2)
      n /= 2
    end
    factors.push(n)
  end
  factors
end
```

Let's run the tests now:

```shell
. F . .

Failures:

  1) prime_factors should return [2] if given 2
     Failure/Error: expect(prime_factors(2)).to eq [2]
     
       expected: [2]
            got: [2, 1]
     
       (compared using ==)
     # ./spec/prime_factors_spec.rb:9:in `block (2 levels) in <top (required)>'

Finished in 0.03591 seconds (files took 0.207 seconds to load)
4 examples, 1 failure
```

We can see the new test passed, but the test that failed is the test number 2. After the nested ```if``` statement we need to avoid to add ```1``` to the array, so we must add another ```if``` statement that checks whether the number is greater than ```1```:

```ruby
# in prime_factors.rb
def prime_factors(n)
  factors = []
  if n > 1
    if n % 2 == 0
      factors.push(2)
      n /= 2
    end
    if n > 1
      factors.push(n)
    end
  end
  factors
end
```

Now, we expect the tests to pass:

```shell
. . . .

Finished in 0.00284 seconds (files took 0.09651 seconds to load)
4 examples, 0 failures
```

However, that new ```if``` statement is an interesting one, as it is the same as the one above it. Since we have two ```if``` statements in the row, we can take the second one and move it completely out of the loop and the tests still pass:

```ruby
# in prime_factors.rb
def prime_factors(n)
  factors = []
  if n > 1
    if n % 2 == 0
      factors.push(2)
      n /= 2
    end
  end
  if n > 1
    factors.push(n)
  end
  factors
end
```

We have now two ```if``` statements in the row with the same predicate and you can see how the last ```if``` statement looks like the end condition of a ```while``` loop. However, let's move on for now.

### Prime Factors of 5

Next test. The prime factors of ```5``` is an array with the ```5``` in it:

```ruby
# in prime_factors.rb
it "should return [5] if given 5" do
  expect(prime_factors(5)).to eq [5]
end
```

We expect the new test to pass:

```shell
. . . . .

Finished in 0.00282 seconds (files took 0.09823 seconds to load)
5 examples, 0 failures
```

### Prime Factors of 6

Next test. The prime factors of ```6``` is an array with ```2``` and ```3``` in it:

```ruby
# in prime_factors.rb
it "should return [2, 3] if given 6" do
  expect(prime_factors(6)).to eq [2, 3]
end
```

We expect the new test to pass:

```shell
. . . . . .

Finished in 0.00775 seconds (files took 0.10306 seconds to load)
6 examples, 0 failures
```

### Prime Factors of 7

Next test. The prime factors of ```7``` is an array with the ```7``` in it:

```ruby
# in prime_factors.rb
it "should return [7] if given 7" do
  expect(prime_factors(7)).to eq [7]
end
```

We expect the new test to pass:

```shell
. . . . . . .

Finished in 0.00652 seconds (files took 0.09748 seconds to load)
7 examples, 0 failures
```

### Prime Factors of 8

Next test. The prime factors of ```8``` is an array with three ```2``` in it:

```ruby
# in prima_factors_spec.rb
it "should return [2, 2, 2] if given 8" do
  expect(prime_factors(8)).to eq [2, 2, 2]
end
```

We expect the new test to fail, because there is nothing in our code that puts three elements in the array:

```shell
. . . . . . . F

Failures:

  1) prime_factors should return [2, 2, 2] if given 8
     Failure/Error: expect(prime_factors(8)).to eq [2, 2, 2]
     
       expected: [2, 2, 2]
            got: [2, 4]
     
       (compared using ==)
     # ./spec/prime_factors_spec.rb:33:in `block (2 levels) in <top (required)>'

Finished in 0.03551 seconds (files took 0.10296 seconds to load)
8 examples, 1 failure
```

How can we get this to pass with the fewest possible keystrokes? We can change the ```if``` statement to a ```while``` loop, that keeps checking whether the number is divisible by ```2```:

```ruby
# in prime_factors.rb
def prime_factors(n)
  factors = []
  if n > 1
    while n % 2 == 0
      factors.push(2)
      n /= 2
    end
  end
  if n > 1
    factors.push(n)
  end
  factors
end
```

We expect the tests to pass:

```shell
. . . . . . . .

Finished in 0.0073 seconds (files took 0.09944 seconds to load)
8 examples, 0 failures
```

What happened there? A ```while``` is a general form of an ```if``` statement. An ```if``` statement is a degenerate form of a ```while``` loop. Again, this is a move towards *generality*. We have made the code more general simply by letting the ```if``` statement continute until it is false, which is interesting. Let's move on to ```9```.

### Prime Factors of 9

The prime factors of ```9``` is an array with two ```3``` in it:

```ruby
# in prime_factors.rb
it "should return [3, 3] if given 9" do
  expect(prime_factors(9)).to eq [3, 3]
end
```

We expect this to fail as there is nothing in our code that puts two ```3``` in it:

```shell
. . . . . . . . F

Failures:

  1) prime_factors should return [3, 3] if given 9
     Failure/Error: expect(prime_factors(9)).to eq [3, 3]
     
       expected: [3, 3]
            got: [9]
     
       (compared using ==)
     # ./spec/prime_factors_spec.rb:37:in `block (2 levels) in <top (required)>'

Finished in 0.0179 seconds (files took 0.10821 seconds to load)
9 examples, 1 failure
```

If we look at the code, we realize that there is a little engine that factors out all the ```2```s. So, we can repeat that loop and factor out all the ```3```s:

```ruby
# in prime_factors.rb
def prime_factors(n)
  factors = []
  if n > 1
    while n % 2 == 0
      factors.push(2)
      n /= 2
    end
    while n % 3 == 0
      factors.push(3)
      n /= 3
    end
  end
  if n > 1
    factors.push(n)
  end
  factors
end
```

We expect the tests to pass:

```shell
. . . . . . . . .

Finished in 0.00703 seconds (files took 0.09514 seconds to load)
9 examples, 0 failures
```

However, this violates the rule of making the code more general. In fact, by duplicating the code, we have made the code more specific. We know what we want to do now. We just need to do it in a more general way.

First thing we need to do is change that ```2``` into a variable called ```divisor```. Then, we need to take that code and put it into another loop, ending when the number is reduced to ```1```. So we will execute this loop *while* the number is greater than ```1```, by changing the first ```if``` statement to a ```while``` loop. Finally, we need to take the initializer outsides the loop and increment it at the end of the loop block.

```ruby
# in prime_factors.rb
def prime_factors(n)
  factors = []
  divisor = 2
  while n > 1
    while n % divisor == 0
      factors.push(divisor)
      n /= divisor
    end
    divisor += 1
  end
  if n > 1
    factors.push(n)
  end
  factors
end
```

We expect the tests to pass:

```shell
. . . . . . . . .

Finished in 0.00735 seconds (files took 0.10681 seconds to load)
9 examples, 0 failures
```

However, there is still some refactoring we can do. The first ```while``` loop cannot exit until ```n``` is equal to ```1```, therefore the following ```if``` statement is not necesary anymore and can be removed.

```ruby
def prime_factors(n)
  factors = []
  divisor = 2
  while n > 1
    while n % divisor == 0
      factors.push(divisor)
      n /= divisor
    end
    divisor += 1
  end
  factors
end
```

We can also change the ```while``` loops to ```for``` loops:

