# Full-Proof Testing: A Guide To Predicable Application

[Presentation](./Presentation.pptx)

## Story time

Let me tell you a story!

A long time ago, I was working as a freelancer. During that time, I didn't know yet what testing was.

When I was finishing my project, I was sending it to the client. I rested for a few days, while he was searching the bugs for me. Then, like a knight battling monsters, I fiercely fought each bug. There was no end to them. The longer I fought, the more bugs were appearing. Some of them became familiar to me, I remembered that I had slain them before.

Then, I discovered E2E testing. It was a breath of fresh air. I would set up tests to look closely if main functionality of the project was still working. And each time I killed a bug, I set up a new test, to watch for its reappearance. The number of bugs decreased, they became easier to fight. And they stopped coming back. My client was happy. But as the project grew, E2E tests became a heavy burden. Their slow nature and big demands dragged me down. Each time I made small changes to the project, I had to wait for hours. I also always had to run a copy of the backend, and maintain a complex system on my laptop.

I decided that I had had enough. My first step was to remove the backend from most of my tests. I replaced it with a mock server, and as a result, I got Functional tests. Now, these tests evaluated most of the application's functionality, while E2E tests focused solely on the connection to the backend. It made the testing system simpler, and decreased execution time from several hours, to less than an hour.

But I wanted more! It was not enough for me. So I started writing unit tests. I was setting up small tests everywhere in the code, and bugs didn't have any place to hide. Those tests were extremely fast, easy to set up, and required few resources. The execution time has been drastically reduced.

I was still using Functional and E2E tests to ensure that the integration between parts of the application and external services was still working as expected. Now, I was able to catch most of the bugs in minutes, and even the most evasive ones were caught in less than ten minutes.

## Me and IDeal

Good evening! My name is Illia, and I am currently working as a frontend developer on iDeal at ABN AMRO. Our testing system is even more advanced, and having it in place allows me to sleep peacefully at night.

## Full proof testing

Today, I will speak about testing, show you different types of tests, and explain various testing methods and their benefits, finishing the presentation with a practical example.

## Types of tests

There are multiple species of bugs. And to catch them all, you need different weapons. Let me tell you about them.

- **Unit tests** - are testing pieces of code separately from each other
- **Functional tests** - are testing your whole application in isolation from external services
- **E2E tests** - are focusing on integration between your application and external services
- **Accessibility tests** check for common problems that may make it difficult for users with disabilities to use your website.
- **Performance tests** will explain you how much time user will take to load the page, how bad network will affect his experience, and what is the load on his processor.
- **Mutation tests** will randomly generate bugs in your code to check if your tests can spot them. Basically they test your tests.
- **Contract tests** are checking that agreement between your frontend and backend is still correct. In constrast to E2E tests, they are fast and executed on unit level.
- **Memory leakage tests** as the name suggests helps you to find memory leaks in your application. If expected session time of your users is long, that is something you definitely want to test for.
- **Snapshots tests** compares confirmed html output with new version. It helps you spot unexpected UI changes.
- **Security tests** checks your code for vulnerabilities.
- **Code quality tests** helps you to ensure same code style withing a team or organisation. Also it shows you common mistakes and problems in your codebase.

## Testing Pyramid

As my story told, tests can be quite expensive. You have to use them sparingly. 

Hense Testing Pyramid is coming into play. It helps you to set correct priorities, and keep the feedback loop as short as possible.

I would start with static tests. It is an one time investment. Once you setup them, they will give you advices during whole life of your application.

Unit tests are fastest and least demanding. Don't be greedy with them.

You can also setup contracts tests, which would be of a great help. But for that you must get cooperation from your backend providers. It can be challenging if they are owned by different teams or even companies.

If unit tests didn't help, use Functional. They are here to ensure correct communication between parts of your application.

And E2E tests will let you know that you application is still producing correct result when it connects to the backend.

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

You will never be able to deploy untested application again. 

## An example

Let's imagine that we are developing a webshop and see with this example how we should approach its testing.

[Wireframe](https://wireframe.cc/PuuV02)

### Pipeline and static tests

First step would be to setup pipeline and static tests. It is one time investment, so once you are done, you will need to make only minor adjustments over lifetime of your application.

### Traceability matrix

Then we define the requirements. Usually traceability matrix is split per page. So we have traceability matrix for catalog, product cart, checkout, and payment.

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

### Unit level

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

Finally, we have properly tested function on a unit level.

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

- Happy Flow without discount - User makes successful payment. In the same test you can test validation.
- Happy Flow with discount
- Error flow on catalog, cart, checkout, and payment page

This should be enough to be reasonably sure that integration between all parts of the application is successful.

`Example picture`

## Functional level (Happy flow example)

How can you write test without having anything to test. There where wishful thinking coming to help. This is an approach of coding, when you use functionality that you have not yet written. Let's see how it works.

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

And last step: E2E tests. It is the best to keep them as simple as possible. So in our case, we just have happy flow.

## QA time

Thank you for your attention. Now the floor opens for a Q&A session. If you think of questions later or want a deeper discussion, I will be around for the entirety of the meetup, so please feel free to approach me. If you'd like to connect or discuss further, I will also share my LinkedIn profile there. Looking forward to your questions and further interractions.