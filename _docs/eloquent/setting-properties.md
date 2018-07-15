---
title: Setting Properties
permalink: /docs/najs-eloquent/setting-properties/
---

## `fillable`

Just like Laravel, you can define mass assignable fields by `fillable` or `guarded` property

```typescript
export class User extends Eloquent {
  protected fillable = ['email', 'username']

  // This way also works :)
  // static fillable = ['email', 'username']
}
```

Only defined properties in fillable can be filled with `.fill()`

```typescript
const user = new User()
user.fill({ email: 'a', password: 'x' })

console.log(user.toObject()) // => { email: a }
```

You can skip `fillable` setting by using `.forceFill()`

## `timestamps`

You can simply define timestamps for models by changing static (or protected) variable named `timestamps`. By using this feature every time the model get saved, `created_at` and `updated_at` will updated automatically

```typescript
export class User extends Eloquent {
  protected timestamps = true

  // This way also works :)
  // static timestamps = true
}
```

By default, Najs Eloquent will create 2 fields named `created_at` and `updated_at`, you can custom the fields' name:

```typescript
export class User extends Eloquent {
  protected timestamps = { createdAt: 'created', updatedAt: 'updated' }

  // This way also works :)
  // static timestamps = { createdAt: 'created', updatedAt: 'updated' }
}
```

## `softDeletes`

You can simply define soft deletes feature for models by changing static (or protected) variable named `softDeletes`.

```typescript
export class User extends Eloquent {
  protected softDeletes = true

  // This way also works :)
  // static softDeletes = true
}
```

By using this feature every time the model get deleted the data ind database not actually deleted, it update `deleted_at` from `null` to date object. You can custom the `deleted_at` field name:

```typescript
export class User extends Eloquent {
  protected softDeletes = { deletedAt: 'deleted' }

  // This way also works :)
  // static softDeletes = { deletedAt: 'deleted' }
}
```

With soft deletes model all retrieve operators like `.find()` or `.get()` automatically return non-deleted entries only, you can use `.withTrashed()` or `.onlyTrashed()` to receive deleted entries