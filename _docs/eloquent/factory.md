---
title: Model Factories
permalink: /docs/eloquent/factory/
---

## What is Model Factories?

Model factories are a way of programmatically making dummy data for testing or seeding. Instead of seeding the certain data and insert into the database, you can use Factory to create random data which look like user input data or you can create full model's data for testing by filling valuable part only, the rest can be generated automatically.

Given that we have an User model like this

```typescript
import { Eloquent, Mongoose } from 'najs-eloquent'

export interface IUser {
  first_name: string
  last_name: string
  email: string
  age: string
}

export class User extends (Eloquent as Mongoose<IUser>) {
}
```

Firstly we have to define the model with Factory definition:

```typescript
import { Factory, Faker } from 'najs-eloquent'
import { User } from './models/User'

Factory.define(User, (faker: Faker, attributes?: Object): Object => {
  return Object.assign(
    {
      email: faker.email(),
      first_name: faker.first(),
      last_name: faker.last(),
      age: faker.age()
    },
    attributes
  )
})
```

After adding a definition for model, in `test` or `seed` file, you can easy create an instance of `User`

```typescript
import { factory } from 'najs-eloquent'

describe('User', function() {
  it ('create and save to database', function() {
    // create and save to the database new user with all random attributes
    const fullRandomlyUser = await factory(User).create()
    
    // create and save new user with your custom email, the rest are generated randomly
    const customEmailUser = await factory(User).create({ email: 'test@test.com' })
  })

  it ('create instance of User which filled by random data', function() {
    // create instance of User (do not save to the database) with all random attributes
    const fullRandomlyUser = factory(User).make()
    
    // create instance of User (do not save to the database) with your custom email
    const customEmailUser = factory(User).make({ email: 'test@test.com' })
  })
})

```

`Najs Eloquent` uses [chance](http://chancejs.com) as a default Faker.