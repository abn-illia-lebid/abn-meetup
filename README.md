# Full-Proof Testing: A Guide To Predicable Application

## About me and IDeal

Nice to see you! I am Illia. 

For last year I was working on the new IDeal as a frontend developer.

I think most of you know what is IDeal. For those who don’t, it is most used online payment system in Netherlands.

For us it was highly important that our application will be able to work properly for most of the users. Each mistake that we would have affected thousands if not millions of users.

That's why we invested a great deal of time into testing.

## Plan for the session

During following 30 minutes I will:

`List item #1 - What is the goal of testing` - Start with explaining the goal of testing. And how it helps you to sleep peacefully at night.

`List item #2 - Testing pyramid` - Then I will speak about Testing Pyramid, the logic behind it, and will briefly cover other testing methods.  

`List item #3 - Traceability matrix` - I will explain what is Traceability Matrix and how it helps you to gain confidence in your tests.

`List item #4 - Unit tests`

`List item #5 - Functional tests`

`List item #6 - E2E tests` - I will cover in depth difference between Unit, Functional and E2E tests, and provide examples.

`List item #7 - Integration with pipeline` - After that I will speak about the importance of integrating tests with the pipeline.

`List item #8 - Breaking the tests` - I will explain how breaking the tests helps you to make them stronger.

`List item #9 - Test Driven Development` - I will cover Test Driven Development, and show with example how it makes tests human readable.

`List item #10 - Questions` - After that I will be open to questions or maybe suggestions.

## What is the goal of testing

Why should you write tests?

I remember that first time when I heard about writting tests, I was working as a freelancer that time, I was thinking: "Why would I ever want to write a test, which proves what I just checked myself". 

Until I started introducing changes to the application that was in production. My client was coming back to me with a list full of bugs that I just fixed last month. They were broken in completely different place in the code, but it was not something I could explain to him.

The principle of tests is simple. They prove that your code is still fullfilling business requirements. Each time that we introduce changes, our tests are asking the code questions like:

- Can user still pay for a product
- Can user still cancel a transaction
- Can user still get the content of the help section
- Will user still be notified if something goes wrong
- Will user still be helped out if he makes a mistake
- And hungreds or thousands more

Manual testing of all those edge cases would have taken hours if not days. But automated tests can do it in a matter of minutes.

## Testing Pyramid

Tests are divided into several levels. Unit level, Functional level, and E2E level.

With unit tests being the fastest. And E2E tests being the slowest. 
You should always aim to cover as much as possible on lower levels of the pyramid. 
And as little as possible on higher levels.

Another way of looking at it is:

- Unit level proves that particular piece of code is working as expected.

- Functional level proves that application has integrated all those pieces correctly

- E2E level is proving that application is successfully connected to third-parties

---

There are a lot of other frontend testing method you may find useful in some situations. I tell in short about some of them, but will not go in details in this presentation.

- **Accessibility testing (example: Axe Deque)** checks for common problems that may make it difficult for users with disabilities to use your website. This particular type of automated testing is not as useful in practice, in my experience it catches only 10% of accessibility problems. Therefore you should always manually check your application for an accessibility.
- **Smoke tests (example: Cypress)** do a quick and basic check of the most important functionality. You should consider it if your tests are taking way too much time. It will quickly show that your application is broken, without a need to run all other tests.
- **Performance tests (example: Google lighthouse)** on the frontend side are different from the backend. While backend performance tests are ensuring that an application can handle high load. Frontend performance tests focus on page load time, javascript executing time, total blocking time or in other words how much your website is lagging, and other user specific metrics.
- **Mutation tests (example: Striker)** randomly change your code: if-else statements, arithmetic operations, removes random blocks and checks if your tests catch produced errors. 
- **Contracts testing (example: Pact)** tests on unit level that contract between provider and consumer is still valid. This way provider can easily modify its API because he knows that it is still compliant with all its consumers (For example with your frontend application). 
- **Memory leakage tests (example: MemLab)** as the name suggests helps you to find memory leaks in your application. If expected session time of your users is long, that is something you definitely want to test for.
- **Snapshots tests (example: Jest)** compares confirmed html output with new version. Helps you spot unexpected UI changes.
- **Security tests (Fortify)** checks your code for vulnerabilities.
- **Code quality tests (SonarQube + Eslint)** helps you to ensure same code style withing a team or organisation. Also shows you common mistakes and problems in your codebase. 

For sure using all of those methods at the same time is not realisticly possible. You have to decide by yourself what is going to give the most benefits in your case.

## Traceability matrix

The goal of the tests as whole, is to prove that application is working as expected from user perspective.

To ensure that, we should use Traceability Matrix. 
It is a list of all business requirements of an application.

The first step would be to identify all the business requirements.

Few examples of such a requirement:

- “When user fills an input incorrectly, validation message should appear”.

- “When user clicks cancel, confirmation modal should appear”

- “When user confirms cancelation, he should be redirected back to the webshop”

During the development we should go back to the traceability matrix, and ensure that all such requirements are met. 
I will explain how you can ensure that business requirements is properly tested on section "Breaking the tests".


> <br>
> 
> **Example of table, with some of requirements marked as tests, and others as not tested**
> | Requirement                                                                 | Is Tested |
> |-----------------------------------------------------------------------------|-----------|
> | When user fills an input incorrectly, validation message should appear      | NO        |
> | When user clicks cancel, confirmation modal should appear                   | YES       |
> | When user confirms cancelation, he should be redirected back to the webshop | NO        |
> 
> <br>

<br>


### Unit level 

Unit tests are the foundation of your testing strategy. 
They are cheap, fast, inexpensive.
This allows us to cover as much edge cases as we want.

There are multiple tools you can use for that, the list below is not exclusive.

- Jest; Jasmine; Karma; Mocha; Ava (Testing javascript modules)
- Cypress component testing; Storybook (For testing components)
- React testing library; Vue test utils; Vitest (For framework testing)

Usually you gonna choose a combination of those. 
For example we may take Jest + React testing library and Cypress component testing.

> <br>
> 
> **Example of function**
> ```javascript
> function calculateDiscount(price) {
>	  if(!price) throw new Error('PriceIsRequiredParameter');
>	  if(!isNumber(price)) throw new Error('NotANumber');
>	  if(price < 0) throw new Error('NegativeNumber');
>
>	  if(price === 0) return 0;
>
>	  if(price >= 1000) return price * 0.25;
>
>	  if(price >= 500) return price * 0.1;
>
>	  if(price >= 200) return price * 0.05;
>
>	  return 0;
> }
> ```
>
> <br>

<br>

Let's go to the example. Imagine that we have following function, which is calculating the discount based on the price which we have provided.

**It should:**
- give 5% discount if price is 200
- give 5% discount if price is between 200 and 500
- give 10% discount if price is 500
- give 10% discount if price is between 500 and 1000
- give 25% discount if price is 1000
- give 25% discount if price is more than 1000
- give 0% discount if price is less than 200
- throw an error if no price is provided
- throw an error if price is less than 0
- throw an error if price is not a number

So what I would expect to test:

- That discount is correctly applied in respective price range. Including edge cases.
- That it throws an error if we provide unexpected parameters.

> <br>
> 
> **Example of modified function**
> ```javascript
> function validatePrice(price) {
>	  if(!price) throw new Error('PriceIsRequiredParameter');
>	  if(!isNumber(price)) throw new Error('NotANumber');
>	  if(price < 0) throw new Error('NegativeNumber');
> }
> 
> function calculateDiscount(price) {
>  	const error = validatePrice(price);
>
> 	if(error) throw error;
>
> 	if(price < 200) return 0;
> 	else if(price < 500) return price * 0.05;
> 	else if(price < 1000) return price * 0.1;
> 	else return price * 0.25;
> }
> ```
>
> <br>

<br>

And now we can safely modify that function without any fear of it breaking. 
Because we know that test will check all functionality of this function each time that we are making any change.

### Functional level

![Web shop example photo](Webshop.png)

While unit tests are focusing on a piece of the code, functional tests are checking your whole application. 

To make an example, let's imagine that we have a webshop. 
You should have already covered all the edge cases in your unit tests.

And now we just want to see that integration of all those pieces of the code, which makes your application, are working fine.

We want to focus on our application at this moment, so we mock API calls. 

I would write several tests to prove that integration between those components works:

- **Happy flow:** User can login, add product to the basket, complete checkout, pays and see success payment page.
- **Error login flow:** When user logins, error message should appear if backend fails with an error and user should be stopped.
- **Basket failure flow:** When user adds product to busket, error message should appear if backend fails with an error and user should be able to try again.
- **Checkout failture flow:** When user submits checkout form, error message should appear if backend fails with an error and user should be able to try again.
- **Payment failure flow:** When user tries to pay, error message should appear if he doesn't have enough money.

### E2E level

Finally, we have to be sure that our application is successfully integrated with API, and whole system is working properly together. 
This is the most expensive, but absolutely necessary part of our testing strategy.

In our example from above, we would have to test only HappyFlow, since login page till SuccessScreen. 

It will prove that connection between all the parties is estabilished.

## Integration with pipeline

We can run all our tests from local machine, but that is prone to human error. 
You can simple forget to run them before deploying it to production. 

Therefore, we should integrated those tests into our pipeline. 
That way each time we push our code, unit and functional tests will be run. 
And when we deploy our application to the testing environment, E2E tests would be run.

If one of the tests will fail, it will stop the pipeline. 
Which will make it impossible to break the production.

## Breaking the tests

Let's speak a bit more about how we can ensure that each requirement of Traceability Matrix is properly tested. 
It is super important, because as long as all the requirements are properly tested, your application would be working fine. 
As long as you have not missed something in the traceability matrix.

We can mark that requirement is tested when we write tests for it. But that would not be enough.
To be sure that your tests are good enough, we should try to break the functionality of an application without triggering tests.
If you cannot do that, then your tests are resilient enough.

> <br>
>
>	**Traceability matrix table, including price example**
> | Requirement                                                                    | Is Tested   |
> |--------------------------------------------------------------------------------|-------------|
> | When user basket price is higher less than 200 Euro - no discount should apply | NO          |
> | When user basket price is higher than 200 Euro - 5% discount should apply      | NO          |
> | When user basket price is higher than 500 Euro - 10% discount should apply     | NO          |
> | When user basket price is higher than 1000 Euro - 25% discount should apply    | NO          |
> 
> <br>

<br>

Let's go back to the function which I showed in unit testing section.
This function has some requirements, which you see on the right of the slide.
Right now all of them are green, which means that they are passing.

> <br>
> 
> **Example of function**
> ```javascript
> function calculateDiscount(price) {
>	  if(!price) throw new Error('PriceIsRequiredParameter');
>	  if(!isNumber(price)) throw new Error('NotANumber');
>	  if(price < 0) throw new Error('NegativeNumber');
>
>	  if(price === 0) return 0;
>
>	  if(price >= 1000) return price * 0.25;
>
>	  if(price >= 500) return price * 0.1;
>
>	  if(price >= 200) return price * 0.05;
>
>	  return 0;
> }
> ```
>
> <br>

> <br>
> 
> **List of requirements**
> It should
> - give 5% discount if price is 200 `Green`
> - give 5% discount if price is between 200 and 500 `Green`
> - give 10% discount if price is 500 `Green`
> - give 10% discount if price is between 500 and 1000 `Green`
> - give 25% discount if price is 1000 `Green`
> - give 25% discount if price is more than 1000 `Green`
> - give 0% discount if price is less than 200 `Green`
> - throw an error if price is not provided `Green`
> - throw an error if price is not a number `Green`
>
> <br>

<br>

Now let's break it a bit. I have remove equal sign from if statements.
If at least one tests has failed, it means that the requirement is properly tested.
In our case for each equal sign we have one test failing, which is good.

> <br>
> 
> **Function example with > instead of >= sign**
> ```javascript
> function calculateDiscount(price) {
>	  if(!price) throw new Error('PriceIsRequiredParameter');
>	  if(!isNumber(price)) throw new Error('NotANumber');
>	  if(price < 0) throw new Error('NegativeNumber');
>
>	  if(price === 0) return 0;
>
>	  if(price > 1000) return price * 0.25;
>
>	  if(price > 500) return price * 0.1;
>
>	  if(price > 200) return price * 0.05;
>
>	  return 0;
> }
> ```
>
> <br>

> <br>
>
> It should
> - give 5% discount if price is 200 `Red`
> - give 5% discount if price is between 200 and 500 `Green`
> - give 10% discount if price is 500 `Red`
> - give 10% discount if price is between 500 and 1000 `Green`
> - give 25% discount if price is 1000 `Red`
> - give 25% discount if price is more than 1000 `Green`
> - give 0% discount if price is less than 200 `Green`
> - throw an error if price is not provided `Green`
> - throw an error if price is not a number `Green`
>
> <br>

---

Now let's fix the mistake, and introduce new one. This time I do not return an error if price is less than 0.
And we see that all tests are green.
Even though we introduced some unexpected changes to the code, no test was alerted. It means that we undertest this function.

> <br>
> 
> **Function example without throwing error if there is no price provided**
> ```javascript
> function calculateDiscount(price) {
>	  if(!price) throw new Error('PriceIsRequiredParameter');
>	  if(!isNumber(price)) throw new Error('NotANumber');
>	  if(price < 0) throw new Error('NegativeNumber');
>
>	  if(price === 0) return 0;
>
>	  if(price >= 1000) return price * 0.25;
>
>	  if(price >= 500) return price * 0.1;
>
>	  if(price >= 200) return price * 0.05;
>
>	  return 0;
> }
> ```
>
> <br>

> <br>
> 
> It should
> - give 5% discount if price is 200 `Green`
> - give 5% discount if price is between 200 and 500 `Green`
> - give 10% discount if price is 500 `Green`
> - give 10% discount if price is between 500 and 1000 `Green`
> - give 25% discount if price is 1000 `Green`
> - give 25% discount if price is more than 1000 `Green`
> - give 0% discount if price is less than 200 `Green`
> - throw an error if price is not provided `Green`
> - throw an error if price is not a number `Green`
>
> <br>

---

Let's add a new test now, which is called "It should throw an error if price is less than zero". Now it is Red.


> <br>
> 
> **Function example without throwing error if there is no price provided**
> ```javascript
> function calculateDiscount(price) {
>	  if(!price) throw new Error('PriceIsRequiredParameter');
>	  if(!isNumber(price)) throw new Error('NotANumber');
>	  if(price < 0) throw new Error('NegativeNumber');
>
>	  if(price === 0) return 0;
>
>	  if(price >= 1000) return price * 0.25;
>
>	  if(price >= 500) return price * 0.1;
>
>	  if(price >= 200) return price * 0.05;
>
>	  return 0;
> }
> ```
>
> <br>

> <br>
>
> It should
> - give 5% discount if price is 200 `Green`
> - give 5% discount if price is between 200 and 500 `Green`
> - give 10% discount if price is 500 `Green`
> - give 10% discount if price is between 500 and 1000 `Green`
> - give 25% discount if price is 1000 `Green`
> - give 25% discount if price is more than 1000 `Green`
> - give 0% discount if price is less than 200 `Green`
> - throw an error if price is not provided `Green`
> - throw an error if price is not a number `Green`
> - throw an error if price is less than 0 `Red`
>
> <br>

---

We return back the line I removed from the code, and now we see that that test we just wrote is now green again.

> <br>
> 
> **Example of function**
> ```javascript
> function calculateDiscount(price) {
>	  if(!price) throw new Error('PriceIsRequiredParameter');
>	  if(!isNumber(price)) throw new Error('NotANumber');
>	  if(price < 0) throw new Error('NegativeNumber');
>
>	  if(price === 0) return 0;
>
>	  if(price >= 1000) return price * 0.25;
>
>	  if(price >= 500) return price * 0.1;
>
>	  if(price >= 200) return price * 0.05;
>
>	  return 0;
> }
> ```
>
> <br>

> <br>
> 
> It should
> - give 5% discount if price is 200 `Green`
> - give 5% discount if price is between 200 and 500 `Green`
> - give 10% discount if price is 500 `Green`
> - give 10% discount if price is between 500 and 1000 `Green`
> - give 25% discount if price is 1000 `Green`
> - give 25% discount if price is more than 1000 `Green`
> - give 0% discount if price is less than 200 `Green`
> - throw an error if price is less than 0 `Green`
> - throw an error if price is not provided `Green`
>
> <br>

Using this method, if you try hard enough to break the requirement you have in the Traceability matrix, and you are not able to do that. Then you can mark it as tested. It would be very hard to introduce an unexpected bug that will break it.

## Test Driven Development and Behaviour Driven Testing

Most likely you have already heard about test driven development and its advantages. Maybe you also heard that should not test implementation, but focus instead on behaviour of the code.

If we combine those two approaches, we get series of advantages.

- **Better code design** - Because we are forced to think about the goal of the code as well as all possible its usage as well as all edge cases before we even start writting the it.
- **Human readable tests** - When we write tests after code is ready, we have a tendency to describe what code is doing from implementation perspective. But if we start with tests, we will be focusing on behaviour. So instead of saying "Click on a button should call updateUser function with following parameters", we would say "Click on a button should update name of the user".
- **Clear documentation** - Because behaviour driven tests are focusing on the functionality of the application, tests in essense serve as a documentation.
- **Cleaner code** - We can easily refactor poorly written or remove unnecessary code without any fear of breaking functionality 
- **Faster development** - Because tests are catching bugs quickly, we spend much less time on debugging. Also, for me tests are working as a GPS. During development we don't have to keep in mind all complex functionality of our application, we just need to pass one specific test at a time.

We convert those business requirements into our code, and then write in human language what we expect to see.

Imagine that we have following requirement: "Form should show an error after user submits the form if entered name is already taken".

I am using wishful thinking together with TDD. So I start by calling functions that don't exist yet, and then write their implementation.

Let's see it in practice. First we would write a test and mount the form:
> <br>
>
>```js
> it(‘should show error message if entered user name is already taken’, () => {
> 	mountWrapper();
> });
> ```
>
> <br>

<br>

Then we have to fill all required fields with user data, so we can submit the form:
> <br>
>
> ```js
> it(‘should show error message if entered user name is already taken’, () => {
> 	mountWrapper();
>      
> 	fillFormWithDummyData();
> });
> ```
>
> <br>

<br>

We change user name to one that is already taken and submit the form.
> <br>
>
> ```js
> it(‘should show error message if entered user name is already taken’, () => {
> 	mountWrapper();
>   
> 	mockSaveUserRequest((request) => {
>	
>   });
>
> 	fillFormWithDummyData();
> 	changeNameTo(‘Illia’);
> 	submitForm();
> });
> ```
>
> <br>

<br>

We expect that the form will send a request to the server. For that we also have to mock server response.

> <br>
>
> ```js
> it(‘should show error message if entered user name is already taken’, () => {
> 	mountWrapper();
>   
> 	mockSaveUserRequest((request) => {
>			if(request.data.name === 'illia') return [500, "NameIsTaken"];
>     
>     return [200, 'Success'];
>   });
>
> 	fillFormWithDummyData();
> 	changeNameTo(‘Illia’);
> 	submitForm();
> });
> ```
>
> <br>

<br>

After that we expect that error message would be displayed
> <br>
>
> ```js
> it(‘should show error message if entered user name is already taken’, () => {
> 	mountWrapper();
>      
> 	fillFormWithDummyData();
> 	changeNameTo(‘Illia’);
> 	submitForm();
> 
> 	expect(getErrorMessage()).toBe(‘Entered user name is already taken’);
> });
> ```
>
> <br>

<br>

And when we enter unique user name and submit it, error message should be hidden, and user name should be changed.

> <br>
>
> ```js
> it(‘should show error message if entered user name is already taken’, () => {
> 	mountWrapper();
>      
> 	fillFormWithDummyData();
> 	changeNameTo(‘Illia’);
> 	submitForm();
> 
> 	expect(getErrorMessage()).toBe(‘Entered user name is already taken’);
> 	
> 	fillFormWithDummyData();
> 	changeNameTo(‘illia.lebid’);
> 	submitForm();
> 
> 	expect(getErrorMessage()).toBe(null);
> 	expect(getUserName()).toBe(‘illia.lebid’);
> });
> ```
>
> <br>

<br>

Usually I write fail statement into each of those functions. And fill them in during the development as I go.

This approach also gives nice level of abstraction. Compare it with following example, which is doing exactly the same as the test which we just wrote.

```js
it(‘should update user name if changes it’, () => {
	const wrapper = mount(UserView, {});

	wrapper.find(‘#input-user-name’).value = ‘Illia’;
	wrapper.find(‘#input-user-name’).trigger(‘input’);

	wrapper.find(‘#input-user-email’).value = ‘illia.lebid@example.com’;
	wrapper.find(‘#input-user-email’).trigger(‘input’);
  wrapper.find(‘#input-password’).value = ‘123456789’;
	wrapper.find(‘#input-password’).value = ‘123456789’;

	wrapper.find(‘#submit-button’).click();

	expect(wrapper.find(‘#error-message’).shadowRoot().querySelector(‘[data-message=“error-name”]’).innerText).toBe(‘Entered user name is already taken’);

	wrapper.find(‘#input-user-name’).value = ‘illia.lebid’;
	wrapper.find(‘#input-user-name’).trigger(‘input’);

	wrapper.find(‘#input-user-email’).value = ‘illia.lebid@example.com’;
	wrapper.find(‘#input-user-email’).trigger(‘input’);
  wrapper.find(‘#input-password’).value = ‘123456789’;
	wrapper.find(‘#input-password’).value = ‘123456789’;

	wrapper.find(‘#submit-button’).click();

	expect(wrapper.find(‘#error-message’).exists()).toBe(false);
	
	expect(wrapper.find(‘#user-name’).text()).toBe(‘illia.lebid’);
});
```

Even if I add comments to make it easier to read, you still would be probably struggling.

```js
it(‘should update user name if changes it’, () => {
	const wrapper = mount(UserView, {});

	// Enter too short user name
	wrapper.find(‘#input-user-name’).value = ‘Illia’;
	wrapper.find(‘#input-user-name’).trigger(‘input’); // input event is necessary for validation

	// Fill form with dummy data
	wrapper.find(‘#input-user-email’).value = ‘illia.lebid@example.com’;
	wrapper.find(‘#input-user-email’).trigger(‘input’); // input event is necessary for validation
  wrapper.find(‘#input-password’).value = ‘123456789’;
	wrapper.find(‘#input-password’).value = ‘123456789’; // input event is necessary for validation

	wrapper.find(‘#submit-button’).click();

	// Shows error message
	expect(wrapper.find(‘#error-message’).shadowRoot().querySelector(‘[data-message=“error-name”]’).innerText).toBe(‘Entered user name is already taken’);

	// Enter valid user name	
	wrapper.find(‘#input-user-name’).value = ‘illia.lebid’;
	wrapper.find(‘#input-user-name’).trigger(‘input’); // input event is necessary for validation

	// Fill form with dummy data
	wrapper.find(‘#input-user-email’).value = ‘illia.lebid@example.com’;
	wrapper.find(‘#input-user-email’).trigger(‘input’); // input event is necessary for validation
  wrapper.find(‘#input-password’).value = ‘123456789’;
	wrapper.find(‘#input-password’).value = ‘123456789’; // input event is necessary for validation

	wrapper.find(‘#submit-button’).click();

	// Doesn’t show eror message
	expect(wrapper.find(‘#error-message’).exists()).toBe(false);
	
	// Renders user name
	expect(wrapper.find(‘#user-name’).text()).toBe(‘illia.lebid’);
});
```

## Finishing presentation



That’s all I wanted to say today. Feel free to reach me out in case you have questions or ideas on how I can improve my testing strategy.

I would be curious to hear your opinion. Thank you, see you next time!

[LinkedIn contacts]