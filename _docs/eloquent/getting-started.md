---
title: Getting Started
permalink: /docs/eloquent/getting-started/
---

## Introduction

The Najs Eloquent ORM inspired by Laravel Eloquent written in typescript. Current version is targeted to Mongodb only 
(using ORM Mongoose as a backer). In the way to `1.0.0`, the Najs Eloquent will support full Eloquent's features with
difference kinds of DB like MySql, PostgreSQL or SqlLite (use knex as a query builder).

## Installation

This section shows you how to setup Najs Eloquent from scratch. Najs already included all setup and 
configuration for Najs Eloquent so if you are using Najs let's skip this part.

Najs Eloquent uses `najs-binding` and `moment` as a peer-dependencies packages, so let's install them along with
najs-eloquent. By default Najs Eloquent use built-in Mongoose instance which connects to mongodb port 27017. Please 
ensure the mongodb is installed and running in your machine. 


```bash
npm install najs-eloquent najs-binding moment --save
```

That's it.

## Using mongoose instance from your application

`mongoose` is singleton so if you want to use with your own instance you have to provide a custom MongooseProvider

```typescript
import { MongooseProvider } from 'najs-eloquent'


```

## Define a model

The Model is very important for every application, it's not only used by server side but also shared with client side.
Najs  Eloquent suggests that you should define an interface for each model which contains model's attribute. For example:

Interface IUser for model User which can be shared cross applications or client-server sides

```typescript
// This interface contains model's attributes only

export interface IUser {
  first_name: string
  last_name: string
  email: string
  password: string
  password_salt: string
  created_at: Date
  updated_at: Date
  deleted_at: Date | null
}
```

There are 3 ways to define a model. They share the same principle/model settings and has it own pros and cons

**The first way**, use `extends Eloquent implements Interface`, you have to copy all attributes definition of `IUser` to
the `User` model

```typescript
import { Eloquent } from 'najs-eloquent'
// import { IUser } from '...'

export class User extends Eloquent implements IUser {
  // By using this way you have to copy alls IUser attributes definition into here
  first_name: string
  last_name: string
  email: string
  password: string
  password_salt: string
  created_at: Date
  updated_at: Date
  deleted_at: Date | null

  // ...
}
```

**The second way**, use `extends Eloquent.Mongoose<Interface, Model>`, this way remember to pass the class as second
generic type

```typescript
import { Eloquent } from 'najs-eloquent'
// import { IUser } from '...'

export class User extends Eloquent.Mongoose<IUser, User>() {
  // ...
}
```

**The third way (suggested)**, use `extends (Eloquent as Mongoose<Interface>)`

```typescript
import { Eloquent, Mongoose } from 'najs-eloquent'
// import { IUser } from '...'

export class User extends (Eloquent as Mongoose<IUser>) {
}
```

| Note - description                     | 1st way | 2nd way | 3rd way (Suggested) |
| :--------------------------------------| :-----: | :-----: | :-----------------: |
| `Instance` queries suggestion            | <i class="fa fa-check text-success"></i> | <i class="fa fa-check text-success"></i> | <i class="fa fa-check text-success"></i> |
| `Static` queries suggestion            |   | <i class="fa fa-check text-success"></i> |   |
| Query log - request level              | <i class="fa fa-check text-success"></i> |   | <i class="fa fa-check text-success"></i> |
| Reduce interface attributes repetition |   | <i class="fa fa-check text-success"></i> | <i class="fa fa-check text-success"></i> |
| No code modified if driver is changed  | <i class="fa fa-check text-success"></i> |   |   |

The principle of `najs` framework is **Learn concept only and write beautiful code with confidential** then the code suggestion is `must-have`. Base on the principle and considering the pros/cons, the 3rd way is recommended.