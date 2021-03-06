# kommit

Format your git commit messages by a set of middlewares.

## Usage

### Install Globally

```sh
npm install -g kommit
kommit
```

### Install Locally

#### install into dependencies

```sh
npm install --save-dev kommit
```

#### add a script into package.json

```json
{
  "script": {
    "kommit": "kommit"
  }
}
```

#### use in terminal

```sh
npm run kommit
```

## Customize

Thanks to the [middleware](https://github.com/IdanLoo/middleware) mechanism, `Kommit` is designed as highly customizable.

### Config File

First of all, to change the behaviour of `kommit`, you should create a folder called `.kommit` in the root of the project. And then add a `config.js` file into it. So the configuration file is placed on the path of `.kommit/config.js`.

### Config Object

The configuration object should be exported in Commonjs pattern.

```js
module.exports = {
  /* ... */
}
```

#### Fields

- members: `String[]`

  The members field should list all the members in your team. So it is able to choose one as your pair. That is for the Pair Programming.

  ```js
  module.exports = {
    members: [
      'Idan Loo <im@siwei.lu>',
      'Fake Name <fake@siwei.lu>',
      /* ... */
    ],
  }
  ```

- hooks: `Object`

  The hooks fields currently contain two hooks. The before hook is a middleware which will be executed before committing, whereas the after hook will be executed after committing.

  ```js
  module.exports = {
    hooks: {
      async before(ctx, next) {
        /* Do something using the ctx */

        // Call the next method to commit this change
        // Avoid calling next to cancel this committing
        await next()
      },
      async after(ctx, next) {
        /* Do something to handle the end of committing */

        // Calling next is essential as well
        await next()
      },
    },
  }
  ```

  To know more things about how to create middlewares, check [here](https://github.com/IdanLoo/middleware)

- scopes: `String[]`

  The scopes field consists of a set of scopes. It will be chosen while committing.

  ```js
  module.exports = {
    scopes: ['middlewares', 'config', 'git', 'ui'],
  }
  ```

### Context

The Context consists of a set of values shared with every middleware.

#### Fields

- path: `String`

  The path you are committing in is `process.cwd()` by default.

- type: `CommitType`

  The type you choosing from a group of commit types. It defined by ACMF includes `feat`, `fix`, `test`, `chore`, `docs`, `refactor`, `style`, `ci` and `pref`.

- subject: `String`

  The subject of this change you filling.

- scope?: `String`

  The scope you choosing from a pre-defined `scopes` in the `config.js`. It will be passed if the `scopes` fields is not provided.

- body?: `String`

  The body describes the details of this committing.

- footer?: `String[]`

  The footer is an array consisting of some extra information of this committing.

- error?: `Error`

  The error is the result of committing. It only can be used in the after hook.

## Summary

Commit Message Format is playing a more and more important role on cooperating development. [Angular Commit Message Format (ACMF)](https://github.com/angular/angular/blob/master/CONTRIBUTING.md#commit-message-format), one of the most well known formats, helps us make git flows clearer and easier to generate documentations.

The Question is, however, this format is a bit complicated. You have to remember lots of concepts and make sure your cooperators know them as well. That does make newbies even some developers who know it well confused.

Anyway, we all know how helpful this format is, and we all are struggling to use it. So why don't we create a tool to make it easier to use?

[git-cz](https://github.com/streamich/git-cz) has done the same thing I want to do. To be honest, `Kommit` is inspired by `git-cz`.

There still are some things I can not handle using `git-cz`. Because my team chooses a variant of ACMF for our demands. But `git-cz` is not such customizable, so I write `Kommit`.
