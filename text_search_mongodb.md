# Mongoose Text Seach

## Define your indexes in the Schema
First add your __text indexes__ in the schema,
you have many alternatives to do this

__Example 1__ `user.js`
 ```js
 var schema = new Schema({
  name: String,
  email: String,
  profile: {
    something: String,
    somethingElse: String
  }
});
schema.index({name: 'text', 'profile.something': 'text'});
 ```
__Example 2__ `user.js`

```js
var schema = new Schema({
  name: String,
  email: {
    type: String,
    index: true
  },
  profile: {
    something: {
      type: String,
      index: true
    },
    somethingElse: String
  }
});

schema.index({ email: 'text'}, {
  email: 'search_email_index',
});

schema.index({ 'profile.something': 'text'}, {
  'profile.something': 'search_profile_some_index'
});

schema.index({ email: 1 });
schema.index({ 'profile.something': 1 });

```
## Find in your model

Now you can use the `$text` operator in your `find queries` to search all fields included in the text index.

__Example 1__

```js
MyModel.find({$text: {$search: searchString}})

```

it's common that your going to need search in many fields so the `$or` is very useful in this scenario, also `$regex` works fine if you want to return documents that looks like the `searchString`

__Example 2__

```js
MyModel.find( {'$or': [
  { '$text': { '$search': searchString } },
  { email: {'$regex': searchString, '$options': '$i' } },
  { phone: {'$regex': searchString, '$options': '$i' } }
 ]})
```

## Realted links

- [$regex](https://docs.mongodb.com/manual/reference/operator/query/regex/)
- [$or](https://docs.mongodb.com/manual/reference/operator/query/or/#op._S_or)
- [Text Index](https://docs.mongodb.com/manual/core/index-text/#create-text-index)
- [Text search ](http://stackoverflow.com/questions/24714166/full-text-search-with-weight-in-mongoose)
- [Full text search stackoverflow](http://stackoverflow.com/questions/28775051/best-way-to-perform-a-full-text-search-in-mongodb-and-mongoose)
