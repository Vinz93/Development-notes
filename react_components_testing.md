## Testing React components

The main idea is to check your isolated components, and make sure that they are rendering in the
correct way, also it's important to validate their behavior too.

---

### Basic concepts

* **describe**: used to group together similar tests
* **it**: used to test single attributes of a target
* **expect**: used to make an *Assertion* about a target
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
### Libraries and links
* Mocha
* Chai
