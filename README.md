# Full-Proof Testing: A Guide To Predicable Application

## Why

Nice to see you! I am Illia. 

Long time ago, I was working as a freelancer. During that time, I didn't know yet what testing is.

When I was finishing my project, I was sending it to the client. I rested few days, while he was searching the bugs for me. Then I fiercely fought with them. There was no end to them. The longer I fought, the more bugs were appearing. Some of them became already familiar to me, I remembered that I have killed them already before.

Then, I discovered E2E testing. It was a breathe of the air. I would set test to look closely if main functionality of the project is still working. And each time I killed a bug, I set new test, to watch for his re-appearence. Amount of bugs decreased, they became easier to fight. And they stopped coming back. My client was happy. But as the project growed, E2E tests became a heavy burden. Their slow nature and big demands dragged me down. Each time I made small changes to the project, I had to wait for hours. I also had to always run a copy of the backend, and maintain complex system on my laptop.

I decided that this was enough for me. My first step was to remove the backend from my tests. I replaced it with mock server, and as a result, I got Functional tests. Now they were testing most of the functionality of my application, and E2E tests were guarding only the connection to the backend. It made the testing system simpler, and saved me several hours each time I introduced changes.

But I wanted more! It was not enough for me So I started writting unit tests. I was settings small tests everywhere in the code, and bugs didn't have any place to hide. Those tests were extremely fast and didn't eat much. I was still using Functional and E2E tests to ensure that the integration between parts of the application and external services is still working as expected. Now, I was able to catch most of the bugs in minutes, and the most evasive ones were caught less than in ten minutes.

## Me and IDeal

Currently I am working on iDeal as a frontend developer in ABN Amro. Our testing system became even more advanced, and having her in place allows me to sleep peacefully at night.

## Plan for the session

During following 20 minutes I will:

`List item #1 - Testing pyramid` - Then I will speak about Testing Pyramid, the logic behind it, and will briefly cover other testing methods.  

`List item #2 - Types of tests` - I will cover shortly most used types of tests, and explain why and when you might want to use them.

`List item #3 - Traceability matrix` - I will explain what is Traceability Matrix and how it helps you to gain confidence in your tests.

`List item #4 - Unit tests`

`List item #5 - Functional tests`

`List item #6 - E2E tests` - I will cover in depth difference between Unit, Functional and E2E tests, and provide examples.

`List item #7 - Integration with pipeline` - After that I will speak about the importance of integrating tests with the pipeline.

`List item #8 - Breaking the tests` - I will explain how breaking the tests helps you to make them stronger.

`List item #9 - Test Driven Development` - I will cover Test Driven Development, and show with example how it makes tests human readable.

`List item #10 - Questions` - After that I will be open to questions or maybe suggestions.

## Types of tests

There are multiple species of bugs. And to catch them all, you need different weapons. Let me tell you about them.

- **Unit tests** - are testing pieces of code separately from each other
- **Functional tests** - are testing your whole application in isolation from external services
- **E2E tests** - are focusing on integration between your application and external services
- **Accessibility tests** check for common problems that may make it difficult for users with disabilities to use your website.
- **Performance tests** will explain you how much time user will take to load the page, how bad network will affect his experience, and what is the load on his processor
- **Mutation tests** will randomly generate bugs in your code to check your tests can spot them. Basically they test your tests.
- **Contract tests** are checking that agreement between your frontend and backend is still correct.
- **Memory leakage tests** as the name suggests helps you to find memory leaks in your application. If expected session time of your users is long, that is something you definitely want to test for.
- **Snapshots tests** compares confirmed html output with new version. Helps you spot unexpected UI changes.
- **Security tests** checks your code for vulnerabilities.
- **Code quality tests** helps you to ensure same code style withing a team or organisation. Also shows you common mistakes and problems in your codebase.

## Testing methods

Weapons are useless if you don't use them properly. Let me tell you about some handy approaches:

- **Testing pyramid**
- **Traceability matrix**
- **Test driven development**
- **Breaking the tests**
- **Integration with the pipeline**

## Testing Pyramid

As my story told, tests can be quite expensive. You have to use them sparingly.

I would start with static tests. It is an one time investment. Once you setup them, they will give you advices during whole life of your application.

Unit tests are fastest and least demanding. Don't be greedy with them.

You can also setup contracts tests, which would be of a great help. But for that you must get cooperation from your backend providers. It can be challenging if they are owned by different teams or even companies.

If unit tests didn't help, use Functional. They are here to ensure correct communication between parts of your application.

And E2E tests will let you know that you application is still producing correct result when it connects to the backend. Be careful with them.

As a last resort, if nothing else helps. Add a test case to your manual testing plan. But be prepared to spend a lot of time in a long run.

## Traceability matrix

Before you start to test anything. You should understand what is expected from your application. As you gather all your requirements, you keep track of their testing status using traceability matrix.

More often than not, if you found a bug, you missed some requirement in the initial stage. So each time you find a bug, check the traceability matrix and update it accordingly.

Another advantage of traceability matrix, is that your clients or stackeholders without technical knowledge will be able to read them.

`Traceability matrix`

## Test Driven Development

Egg or chicken question. What should come first? Tests or code. 

If you look closely, you will see that tests are direct requirements for code. Better let the requirements to shape the code, then otherwise. This approach will also give you human readable tests, will prevent you from writting reduntant code. And main principle (Red -> Green -> Refactor) will give you additional confidence in the tests.

## Breaking the tests

How do you understand that your tests are stable enough?

That's my favorite part.

Break the code! Break the application! Introduce unexpected change! Do whatever you can to fool your tests. Introduce the chaos, and see if they can catch it.

Were you not able to do it? Well, then you can trust your tests.

## Integration with pipeline

What if you forget to run your automated tests and deploy untested application to production?

Worry not, because when you include the tests into the pipeline, they will be run automatically.

The pipeline will also automate the deployment to production. Which gives you even more safety and less headache.

## An example

Let's imagine that we are developing a webshop and see with this example how we should approach its testing.

[Wireframe](https://wireframe.cc/PuuV02)

### Traceability matrix

First we define the requirements. Usually traceability matrix is split per page of the application. So we have traceability matrix for catalog, product cart, checkout, and payment.

Let's briefly go through all of the requirements, to see how you approach the traceability matrix.

----

**Catalog**

On catalog we care that products are displayed, header cart is working properly, and errors are handled.

---

**Product cart**

On product cart we need to render dynamic total price, allow user to change amount of products, apply or disable discounts depending on the total price, make sure that all the changes are persistent and handle errors.

---

**Checkout**

Checkout is a basic form, so it should be validated properly. User should be able to submit it. And errors should be handled.

---

**Payment**

On payment step we care that users can successfully pay for the product, and if they cannot, they would be properly notified.

## Unit level

You can test nearly everything here with unit tests. And you should! You should think of every edge case possible. Think of every possible error that can happen, every possible user behaviour. What if user tries to open empty product cart? What if he removes all the items from the cart? What if he clicks pay button twice? Those are questions that should fill your mind.

As for practical example, let me show how I would write a discount function.

---

First step would be to write tests. In other words to define the requirements to the function.

We expect to get no discount if price is less than 1000Euro and 10% discount is price is more than 1000Euro. Also if price argument is not provided, the function should throw an error.

If you look below the function, you will see the console where we call our function with 10Euro price. It is returning undefined now.

`Unit function with 3/3 failing tests and 10Euro as a parameter`

Let's return unchanged price from the function.

---

First test is passing. We also see that if we call function with 10Euro, we get an expected result. But two other tests are still failing.

`Unit function with 2/3 failing tests and 10Euro as a parameter`

---

Now if we call the function with 5000Euro as a parameter, we also get unchanged price.

`Unit function with 2/3 failing tests and 5000Euro as a parameter`

Let's apply discount if the price is higher than 1000Euro.

---

Second test is passing now. And discount is successfully applied to the price we provided.

`Unit function with 1/3 failing test and 5000Euro as a parameter`

---

If we don't pass a price parameter, then we receive undefined instead of expected error.

`Unit function with 1/3 failing test and no parameters`

Let's fix it.

---

Now you see that all tests are passing, and function is throwing expected error.

`Unit function with 0/3 failing tests and no parameters`

Time to break the code and tests. What will happen if I pass string instead of number as a parameter?

---

It will return unchanged string.

`Unit function with 0/3 failing tests and string parameter`

That is unexpected behaviour. We should add additional test case. Function should throw an error if non-numeric parameter has been passed.

---

It fails. Good.

`Unit function with 1/4 failing tests and string parameter`

Let's fix it.

---

Now function is throwing an error and all tests are passing.

`Unit function with 0/4 failing tests and string parameter`

Can we break something else? We see edge case in 'if' statement.

---

Let's pass 1000Euro to the function. It applies discount. What will happen if we remove equal sign from 'if' statement?

`Unit function with 0/4 failing tests and 1000Euro as a parameter, discount is applied`

---

Discount is not applied now, but no test is failing. We have two different behaviours which pass all the tests. Clear sign that we need an additional test case.

`Unit function with 0/4 failing tests and 1000Euro as a parameter, discount is not applied`

---

Now the when the test is indicating the problem.

`Unit function with 1/5 failing tests and 1000Euro as a parameter, discount is not applied`

Let's fix it.

---

Now we have properly tested function on a unit level.

`Unit function with 0/5 failing tests and 1000Euro as a parameter, discount is applied`

Short recap of what we did:

- We defined requirements to the function
- Wrote the failing tests
- Passed the tests
- Found missed requirement
- Added a failing test case and fixed it.
- Broke the code without failing tests
- Added additional failing test case
- Fixed it

## Functional level

To write our functional tests, you need to zoom out and look at our example from bird view. You don't need to go in details, while testing on functional level, but you shouldn't be afraid of double testing some functionality that you already tested on a lower level.

What I would focus on here, is:

- Happy Flow - User makes successful payment. In the same test you can test validation.
- Happy Flow with discount
- Error flow on catalog, cart, checkout, and payment page

This should be enough to be reasonably sure that integration between all parts of the application is successful.

`Example picture`

## Functional level (Happy flow example)

How can you write test without having anything to test. There where wishful thinking coming to help. This is an approach of testing, when you use functionality that you have not yet written. Let's see how it works.

`Test without anything`

First step of the test, would be to load the catalog page.

---

We use Catalog class, which doesn't exist yet. And call non-existing loadPage method. Because we wish it to be there. You can actually create this class and test, and make it always fail. Then when functionality is being added, you can fix all the failing tests.

`Test with Catalog.loadPage method`

The catalog should display products.

---

`Test with Catalog.showsProduct method`

Header cart should show "no products" message, because it is empty.

---

`Test with HedaerCart.shows('No products')`

When user adds a product, header cart should show counter and total price.

---

`Test with one product in cart`


When user adds several more products, header cart should be updated

---

`Test with 3 products in the cart`

When user clicks on header cart, products cart page should be shown 

---

`Test with products cart`

When user modifies products amount, total price should be updated

---

`Test with updated amount of products in products cart`

When user goes to checkout, submits a form and pays, success page should be displayed.

---

`Complete test`

Now we have complete happy flow, that will guide you through your development process.

## E2E level



## QA time

Thank you for your attention. Now the floor opens for a Q&A session. If you think of questions later or want a deeper discussion, I will be around for the entirety of the meetup, so please feel free to approach me. Additionally, I will be posting slides of this presentation in the meetup comments for reference. If you'd like to connect or discuss further, I will also share my LinkedIn profile there. Looking forward to your questions and further interractions.