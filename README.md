# Adonis Prisma

> Prisma Provider for AdonisJS

[![npm-image]][npm-url] [![license-image]][license-url] [![typescript-image]][typescript-url]

If you want to use [Prisma](https://prisma.io) on AdonisJS, this package provides you with Prisma Client Provider and Auth Provider.

## Getting Started

### Installation

Make sure you've already installed Prisma related packages:

```sh
npm i --save-dev prisma && npm i @prisma/client
```

Install this package:

```sh
npm i @wahyubucil/adonis-prisma
```

### Setup

```sh
node ace configure @wahyubucil/adonis-prisma
```

It will install the provider, and add typings.

## Usage

### Prisma Client Provider

Import the Prisma Client from `@ioc/Adonis/Addons/Prisma`. For example:

```ts
import { HttpContextContract } from '@ioc:Adonis/Core/HttpContext'
import prisma from '@ioc:Adonis/Addons/Prisma'

export default class UsersController {
  public async index({}: HttpContextContract) {
    const users = await prisma.user.findMany()
    return users
  }
}
```

### Authentication (Prisma Auth Provider)

Make sure you read the [AdonisJS Authentication guide](https://docs.adonisjs.com/guides/auth/introduction) first, and configure the Adonis Auth.

You don't need to configure Lucid, if it's auto-generated, remove all Lucid-related files like Model and Migration, also Lucid-related configs on `contracts/auth.ts` and `config/auth.ts`.

After configure the Adonis Auth, you need to config Prisma Auth Provider on `contracts/auth.ts` and `config/auth.ts`. For example:

```ts
// contracts/auth.ts
import { PrismaAuthProviderContract, PrismaAuthProviderConfig } from '@ioc:/Adonis/Addons/Prisma'
import { User } from '@prisma/client'

declare module '@ioc:Adonis/Addons/Auth' {
  interface ProvidersList {
    user: {
      implementation: PrismaAuthProviderContract<User>
      config: PrismaAuthProviderConfig<User>
    }
  }

  ......
}
```

```ts
// config/auth.ts
import { AuthConfig } from '@ioc:Adonis/Addons/Auth'

const authConfig: AuthConfig = {
  guard: 'api',

  guards: {
    api: {
      driver: 'oat',
      provider: {
        driver: 'prisma',
        identifierKey: 'id',
        uids: ['email'],
        model: 'user',
      },
    },
  },
}

export default authConfig
```

Following is the list of all the available configuration options.

```ts
{
  provider: {
    driver: 'prisma',
    identifierKey: 'id',
    uids: ['email'],
    model: 'user',
    hashDriver: 'argon',
  }
}
```

#### driver

The driver name must always be set to `prisma`.

---

#### identifierKey

The `identifierKey` is usually the primary key on the configured model. The authentication package needs it to uniquely identify a user.

---

#### uids

An array of model columns to use for the user lookup. The `auth.login` method uses the `uids` to find a user by the provided value.

For example: If your application allows login with email and username both, then you must define them as `uids`. Also, you need to define the model column names and not the database column names.

---

#### model

The model to use for user lookup.

---

#### hashDriver

The driver to use for verifying the user password hash. It is used by the `auth.login` method. If not defined, we will use the default hash driver from the `config/hash.ts` file.

---

## TODO

- Maybe Seeder implementation?

[npm-image]: https://img.shields.io/npm/v/@wahyubucil/adonis-prisma.svg?style=for-the-badge&logo=npm
[npm-url]: https://npmjs.org/package/Anonymous 'npm'
[license-image]: https://img.shields.io/npm/l/@wahyubucil/adonis-prisma?color=blueviolet&style=for-the-badge
[license-url]: LICENSE.md 'license'
[typescript-image]: https://img.shields.io/badge/Typescript-294E80.svg?style=for-the-badge&logo=typescript
[typescript-url]: "typescript"

```

```
