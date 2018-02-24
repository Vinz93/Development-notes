## Testing React components

The main idea is to check your isolated components, and make sure that they are rendering in the
correct way, also it's important to validate their behavior too.

---

### Basic concepts

* **describe**: used to group together similar tests.
* **it**: used to test single attributes of a target.
* **expect**: used to make an *Assertion* about a target.
* **beforeEach**: It's a function that is going to run before each `it` statement,
 this allows you to set up things that your tests may need.
---
### Syntax
1. The thing we want to make an assertion about.
2. Assertion matcher, how to compare the two given values.
3. the value that we expect.

```js
          1             2         3
expect(component).to.have.class('on');
```
---
### How you should tests

 You should made assertions about the end product, and validate the correct rendering (html & behavior), keep in mind that you have to make sure that your test allows other developers to refactor your code as much as possible.

 Ask yourself a very simple question, what do I expect to happen.

---
### Expecting Child Elements

First, you have to make sure that your application is rendering the comments that you want, this is very easy to check. see the example below.

```js
component = renderComponent(App);
it('shows a comment box', () => {
  expect(component.find('.comment-box')).to.exist;
});
```
In the example above, `component` has the whole app inside and uses the `chaiJquery` plugin, so you are abl to find the components inside the app by his `ClassName` and check for the existence with **to.exist** assertion, this a good reason to always write a class for each component.

---
### Simulating Events
You can use chaiJquery to simulate events in html elements, in order to verify the correct behavior inside your components.

The test below simulates when a user is typing something inside a `textarea`, we expect that the text that was typed by the user to be inside the `textarea`. you can simulate the action of typing with a `change` event, then check the text inside the `textarea.`

```js
it('shows that text in the textarea', () => {
  component.find('textarea').simulate('change','new comment');
  expect(component.find('textarea')).to.have.value('new comment');
});
```
eg. simulating a `submit` event.

```js
it('when submitted, clears the input', () => {
  component.simulate('submit');
  expect(component.find('textarea')).to.have.value('');
});
```
---
### Expectation on Content
There are many different ways to make assertion in the component, it depends in your imagination, you can
always check for the `chai` documentation and look for an assertion that fits in what you want to test.

The example below shows two test for a **commentList**, first it adds the comments and then checks if the quantity of `<li>` is the same as the comments that were pass to the comment via props, the second test a verifies if the comments are the same.

```js
describe('CommentList', () => {
  let component;
  beforeEach(() => {
    const props = { comments: ['so cool', 'get in', 'get out'] };
    component = renderComponent(CommentList, null, props);
  })
  it('shows an LI for each comment', () => {
    expect(component.find('li').length).to.equal(3);
  });

  it('shows each comment that is provided', () => {
      expect(component).to.contain('so cool');
      expect(component).to.contain('get in');
      expect(component).to.contain('get out');
  });
})
```
---
## Redux side Testing
### Testing Action Creators
 - check the correct type.
 - check the payload of the action.
### Testing Reducers
- Test every action that the reducer handles.


---
### Libraries and links
* Mocha
* Chai
`chai-jq` is an alternate plugin for the Chai assertion library to provide jQuery-specific assertions.
