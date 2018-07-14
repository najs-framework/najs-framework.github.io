---
title: Setting Properties
permalink: /docs/najs-eloquent/setting-properties/
---

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
