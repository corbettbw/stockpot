# Stockpot

`Stockpot` makes setting up test data in your Rails database from an external resource easier.

[![Gem Version](https://badge.fury.io/rb/stockpot.svg)](https://badge.fury.io/rb/stockpot)
<!-- [![Build Status](https://semaphoreci.com/api/v1/freshly/stockpot/branches/master/badge.svg)](https://semaphoreci.com/freshly/stockpot)
[![Maintainability](TODO)](https://codeclimate.com/github/Freshly/stockpot/maintainability)
[![Test Coverage](TODO)](https://codeclimate.com/github/Freshly/stockpot/test_coverage) -->

`Stockpot` gives you an easy way to expose a number of end points from your app, enabling CRUD actions that you can utilize from things like a standalone test suite to set up state. For instance, rather than going through the entirety of a new user creation flow just to check that users can update their data once registered, simply make a POST call to `/stockpot/database_management` with your dummy user's data, shortcutting that unnecessary setup and enabling you to go to directly checking the system behavior needed.

Use it in your external Cypress or Cucumber tests, set up a webpage to allow manual testers to set up and get state with an interface fronting the API, etc.

Why the name `Stockpot`? Keeping with Freshly's food related naming, a [stockpot](https://en.wikipedia.org/wiki/Stock_pot) is one of the most common types of cooking pots worldwide, and a stockpot is traditionally use to make stock or broth which can be the basis for cooking more complex recipes. You put ingredients in, do some cooking, and take out a finished product. For this project, think of your database as being the stockpot, putting in test data, doing some action in the system under test, and then pulling out data to use in your assertions of your system's behavior.

### Version 0.1.x Notes

Initial development notes: Stockpot is very much a work in progress. The initial implementation is _mostly_ an abstraction of Freshly's current usage of controllers to allow our external Cypress test project to interact with our Rails app's database. The current pie in the sky plan is to genericize things more so that different tools can be used and allow folk's more configuration options.

* [Installation](#installation)
* [Usage](#usage)
* [Development](#development)
* [Contributing](#contributing)
* [License](#license)

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'stockpot'
```

And then execute:

```bash
bundle
```

Or install it yourself as:

```bash
gem install stockpot
```

## Usage
---

## Rails App

## !! Warning !!

**You should only enable this in environments that are **NOT** production. If you choose to ignore this warning, do so at your own risk!! Wrap the following in a check for environments with something like `Rails.env.test?` if you don't have anything else in place.**

Add the `Stockpot` engine to your `/config/routes.rb` file, changing the base path if you'd like to:

```ruby
mount Stockpot::Engine, at: "/stockpot"
```

This will give you the following [routes](/config/routes.rb) (assuming the default "/stockpot" path):

### `/stockpot/records`

#### GET

Query for data. Accepts a array of objects that require at least a model name, but can also include additional qualifying data.

```javascript
[
  { model: "user" },
  { model: "address" }
]
// or
[
  { model: "user", id: "foo"
]
```

#### POST

Create new data. Accepts an object specifying a single model with additional qualifiers.

* factory (required) - Specify which factory to create a record with.
* list (optional) - Specify a count of items to create, default: 1
* traits (optional) - An array of strings specifying any traits you may want to use with your data. Applies to all records created.
* attributes (optional) - An array of objects allowing for the specification of data to be used when creating records. Array position matches with list order if multiple records are desired.

```javascript
{
  factory: "user",
  traits: ["having_address"],
  attributes: [{
    email: "foo@bar.com",
    password: "baz",
    first_name: "Foo",
    last_name: "Bar"
  }]
}
```

#### DELETE

Remove data. Accepts the same type of input as GET

#### PUT

Update data Accepts an array of objects with each object specifying the model type to be updated along with the data to update. Input is uses the same base as GET and DELETE, but also accepts an `update` object allowing for the specification of the data to be updated.

```javascript
[
  {
    model: "user",
    user_id: "123",
    update: {
      status: "active",
      email: "foo@bar.com"
    }
  }
]
```

### `/stockpot/clean_database`

#### DELETE

Clears Rails & Redis caches and truncates Active Records databases. No body required.

### `/stockpot/redis`

#### GET
  
Query for data. Accepts key or field to use to search cache for record.

```javascript
{
  key: "123",
  field: "foo"
}
```

#### POST - Create new data

Accepts an object specifying a key, field, and value to be inserted into Redis cache.

```javascript
{
  key: "123",
  field: "foo",
  value: "bar"
}
```

## Cypress/External Suite Examples

We utilize Cypress commands to abstract out helpers for setting up state such as

```javascript
Cypress.Commands.add("getRecords", args => {
  cy.request({
    method: "GET",
    url: "stockpot/records",
    body: {
      models: args
    }
  })
})
```

Our tests can then call this command like this
  
```javascript
cy.getRecords([{ model: "user", id: user.id }])
  .then(res => {
    expect(newEmail).to.eql(res.body.users[0].email)
  })
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. To install this gem onto your local machine, run `bundle exec rake install`.

See the next section on contributing for more info.

## Contributing

This Open Source is supported by [Freshly](https://freshly.com), a company committed to quality code and delicious food.

We're basically [always hiring](https://jobs.lever.co/freshly).

Come join us in our New York City, Phoenix, or Minsk offices and write some awesome software!

Community support is always appreciated! Bug reports and pull requests are welcome on [GitHub](https://github.com/Freshly/stockpot).

Please read our [CONTRIBUTING](CONTRIBUTING.md) doc for more information!

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
