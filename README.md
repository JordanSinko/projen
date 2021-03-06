# projen

Define and maintain complex project configuration through code.

> JOIN THE [#TemplatesAreEvil] MOVEMENT!

[#TemplatesAreEvil]: https://twitter.com/search?q=%23TemplatesAreEvil

*projen* synthesizes project configuration files such as `package.json`,
`tsconfig.json`, `.gitignore`, GitHub Workflows, eslint, jest, etc from a
well-typed definition written in JavaScript.

As opposed to existing templating/scaffolding tools, *projen* is not a one-off
generator. Synthesized files should never be manually edited (in fact, projen
enforces that). To modify your project setup, users interact with rich
strongly-typed class and execute `projen` to update their project configuration
files.

## Getting Started

To create a new project, run the following command and follow the instructions:

```console
$ mkdir my-project
$ cd my-project
$ git init
$ npx projen new PROJECT-TYPE
🤖 Synthesizing project...
...
```

Currently supported project types (use `npx projen new` without a type for a
list):

<!-- <macro exec="node ./scripts/readme-projects.js"> -->
* [awscdk-app-ts](https://github.com/eladb/projen/blob/master/API.md#projen-awscdktypescriptapp) - AWS CDK app in TypeScript.
* [awscdk-construct](https://github.com/eladb/projen/blob/master/API.md#projen-awscdkconstructlibrary) - AWS CDK construct library project.
* [cdk8s-construct](https://github.com/eladb/projen/blob/master/API.md#projen-constructlibrarycdk8s) - CDK8s construct library project.
* [jsii](https://github.com/eladb/projen/blob/master/API.md#projen-jsiiproject) - Multi-language jsii library project.
* [node](https://github.com/eladb/projen/blob/master/API.md#projen-nodeproject) - Node.js project.
* [project](https://github.com/eladb/projen/blob/master/API.md#projen-project) - Base project.
* [typescript](https://github.com/eladb/projen/blob/master/API.md#projen-typescriptproject) - TypeScript project.
* [typescript-app](https://github.com/eladb/projen/blob/master/API.md#projen-typescriptappproject) - TypeScript app.
<!-- </macro> -->

> Use `npx projen new PROJECT-TYPE --help` to view a list of command line
> switches that allows you to specify most project options during bootstrapping.
> For example: `npx projen new jsii --author-name "Jerry Berry"`.

The `new` command will create a `.projenrc.js` file which looks like this for
`jsii` projects:

```js
const { JsiiProject } = require('projen');

const project = new JsiiProject({
  authorAddress: "elad.benisrael@gmail.com",
  authorName: "Elad Ben-Israel",
  name: "foobar",
  repository: "https://github.com/eladn/foobar.git",
});

project.synth();
```

This program instantiates the project type with minimal setup, and then calls
`synth()` to synthesize the project files. By default, the `new` command will
also execute this program, which will result in a fully working project.

Once your project is created, you can configure your project by editing
`.projenrc.js` and re-running `npx projen` to synthesize again.

> The files generated by _projen_ are considered an "implementation detail" and
> _projen_ protects them from being manually edited (most files are marked
> read-only, and an "anti tamper" check is configured in the CI build workflow
> to ensure that files are not updated during build).

For example, to setup PyPI publishing in `jsii` projects, you can use
[`python option`](https://github.com/eladb/projen/blob/master/API.md#projen-jsiipythontarget):

```js
const project = new JsiiProject({
  // ...
  python: {
    distName: "mydist",
    module: "my_module",
  }
});
```

Run:

```shell
npx projen
```

And you'll notice that your `package.json` file now contains a `python` section in
it's `jsii` config and the GitHub `release.yml` workflow includes a PyPI
publishing step.

We recommend to put this in your shell profile, so you can simply run `pj` every
time you update `.projenrc.js`:

```bash
alias pj='npx projen'
```

Most projects support a `start` command which displays a menu of workflow
activities:

```shell
$ yarn start
? Scripts: (Use arrow keys)

  BUILD
❯ compile          Only compile
  watch            Watch & compile in the background
  build            Full release build (test+compile)

  TEST
  test             Run tests
  test:watch       Run jest in watch mode
  eslint           Runs eslint against the codebase

  ...
```

The `build` command is the same command that's executed in your CI builds. It
typically compiles, lints, tests and packages your module for distribution.

## Features

Some examples for features built-in to project types:

* Fully synthesize `package.json`
* Standard npm scripts like `compile`, `build`, `test`, `package`
* eslint
* Jest
* jsii: compile, package, api compatibility checks, API.md
* Bump & release scripts with CHANGELOG generation based on Conventional Commits
* Automated PR builds
* Automated releases to npm, maven, NuGet and PyPI
* Mergify configuration
* LICENSE file generation
* gitignore + npmigonre management
* Node "engines" support with coupling to CI build environment and @types/node
* Anti-tamper: CI builds will fail if a synthesized file is modified manually

## API Reference

See [API Reference](./API.md) for API details.

## Ecosystem

_projen_ takes a "batteries included" approach and aims to offer dozens of different project types out of
the box (we are just getting started). Think `projen new react`, `projen new angular`, `projen new java-maven`,
`projen new awscdk-typescript`, `projen new cdk8s-python` (nothing in projen is tied to javascript or npm!)...

Adding new project types is as simple as submitting a pull request to this repo and exporting a class that
extends `projen.Project` (or one of it's derivatives). Projen automatically discovers project types so your
type will immediately be available in `projen new`.

## Contributing

Contributions of all kinds are welcome!

To check out a development environment:

```bash
$ git clone git@github.com:eladb/projen
$ cd projen
$ yarn
```

## License

Distributed under the [Apache-2.0](./LICENSE) license.
