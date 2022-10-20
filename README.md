# testify
[![npm (scoped)](https://img.shields.io/npm/v/@sutro-dev/testify?style=for-the-badge)](https://www.npmjs.com/package/@sutro-dev/testify)
[![GitHub Workflow Status](https://img.shields.io/github/workflow/status/SutroOrg/testify/CI?style=for-the-badge)](https://github.com/SutroOrg/testify/actions/workflows/ci.yml)
[![node-lts (scoped)](https://img.shields.io/node/v-lts/@sutro-dev/testify?style=for-the-badge)](https://nodejs.org/en/download/)


Sometimes you have a function in a file that is private, but needs to be tested.

Exporting it into a production environment can result in pierced abstractions, which defeats the purpose of abstraction.

`testify` allows you to export a wrapped version of a private function. While the function is still exported, attempts to use
it in production will result in a thrown Error.

## Example

```typescript
// library.ts
const privateFunction = () => {
    // Something happens here
};

const _testableFunction = testify(privateFunction);

export {_testableFunction};
```

`_testableFunction` can be used in your tests (i.e. when `process.env.NODE_ENV === 'test'`), but will throw in production.

## Why not use `rewire`?

The [rewire package](https://www.npmjs.com/package/rewire) approaches this problem via monkey-patching. The problem with that is 
that you lose visibility of code coverage on your rewired functions.

This approach allows you to get the coverage data you need.
