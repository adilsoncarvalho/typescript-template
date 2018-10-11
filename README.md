# typescript-template

## Some considerations

- Audience

This template was made for libraries and other general node apps. If
you want to use it with React, I recommend you to read [this document][2].

- Project structure

This project aims to use the following file structure:

```text
--+-- src (our code)
  +-- lib (the transpiled code)
```

- Plugins for your Visual Studio Code

* vscode-tslint
* prettier

## Build Steps

You don't have to follow them. They were used to generate all the
contents of this repository. They are here just for reference.

- Installing `babel7`

```sh
yarn add --dev \
  @babel/core @babel/cli @babel/node \
  @babel/preset-env
```

Create the `.babelrc.js` at the project root as follows

```js
const presets = [];
const plugins = [];

module.exports = { presets, plugins };
```

- Installing `typescript`

```sh
yarn add --dev \
  typescript \
  @babel/plugin-proposal-class-properties \
  @babel/plugin-proposal-object-rest-spread \
  @babel/preset-typescript \
  @types/node
```

Create the `tsconfig.json`

```sh
node_modules/.bin/tsc \
  --init --declaration --allowSyntheticDefaultImports \
  --target esnext --outDir lib
```

Add the following lines at your `scripts` section on your
`package.json` file.

```json
{
  "scripts": {
    "build": "yarn build:types && yarn build:js",
    "build:types": "tsc --emitDeclarationOnly",
    "build:js": "babel src --out-dir lib --extensions \".ts,.tsx\" --source-maps inline",
    "start": "babel-node --extensions \".ts,.tsx\" src/index.ts"
  }
}
```

- Installing `tslint`

```sh
yarn add --dev tslint tslint-config-airbnb
```

Create the `tslint.json` at the project root as follows

```json
{
  "defaultSeverity": "error",
  "extends": ["tslint-config-airbnb"],
  "jsRules": {},
  "rules": {},
  "rulesDirectory": [],
  "linterOptions": {
    "exclude": ["**/node_modules/**"]
  }
}
```

Add the following lines at your `scripts` section on your
`package.json` file.

```json
{
  "scripts": {
    "tslint": "tslint 'src/**/*.ts'",
    "tslint:fix": "tslint --fix 'src/**/*.ts'"
  }
}
```

- Installing `prettier`

```sh
yarn add --dev prettier pretty-quick husky
```

Add the following lines at your `scripts` section on your
`package.json` file.

```json
{
  "scripts": {
    "precommit": "pretty-quick --staged",
    "prettier": "pretty-quick"
  }
}
```

- Installing `jest`

Add to your `package.json` file the following block:

```json
"resolutions": {
  "babel-core": "^7.0.0-bridge.0"
},
```

Then you must add the following dependencies

```sh
yarn add --dev babel-core@^7.0.0-bridge.0 jest ts-jest
```

Add the following lines at your `scripts` section on your
`package.json` file.

```json
{
  "scripts": {
    "test": "jest",
    "test:coverage": "jest --coverage",
    "test:watch": "jest --watch",
    "test:watch:all": "jest --watch --all"
  }
}
```

After that you must invoke `jest` to create its configuration file

```sh
yarn test --init
```

> Notice: this project was set up for `node` and with some choices
> that we like best. Maybe you should run this command anyway so you
> can chance its options to your preferences.

You may get the following output when you invoke `yarn test`:

```sh
ts-jest[backports] (WARN) "[jest-config].globals.ts-jest.tsConfigFile" is deprecated, use "[jest-config].globals.ts-jest.tsConfig" instead.
ts-jest[backports] (WARN) Your Jest configuration is outdated. Use the CLI to help migrating it: ts-jest config:migrate <config-file>.
```

To fix it, just open your `jest.config.js` and replace the `tsConfigFile`
entry for `tsConfig`. Just that. The `config:migrate` does nothing :sweat_smile:

[0]: https://iamturns.com/typescript-babel/
[1]: https://github.com/Microsoft/TypeScript-Babel-Starter
[2]: https://github.com/Microsoft/TypeScript-Babel-Starter#using-jsx-and-react
