# TypeScript and JavaScript lib generator

AppVeyor Status: [![Build status](https://ci.appveyor.com/api/projects/status/8oj3j7u6nvag1xvu/branch/master?svg=true)](https://ci.appveyor.com/project/zhengbli/tsjs-lib-generator/branch/master)
Travis CI Status: [![Build Status](https://travis-ci.org/Microsoft/TSJS-lib-generator.svg?branch=master)](https://travis-ci.org/Microsoft/TSJS-lib-generator)

This tool is used to generate `dom.generated.d.ts`, `webworker.generated.d.ts` and `dom.iterable.generated.d.ts` for TypeScript.
The input file is the JSON webidl file generated by the Microsoft Edge browser.

## Build Instructions

To get things setup:

```sh
npm install
```

To generate the `.d.ts` files

```sh
npm run build
```

To test:

```sh
npm run test
```

## Contribution Guidelines

The `dom.generated.d.ts`, `webworker.generated.d.ts` and `dom.iterable.generated.d.ts` files from the TypeScript repo are used as baselines.
For each pull request, we will run the script and compare the generated files with the baseline files.
In order to make the tests pass, please update the baseline as well in any pull requests.

For common changes, it is sufficient to change the json files.
There are three json files that are typically used to alter the type generation: `addedTypes.json`, `overridingTypes.json`, and `removedTypes.json`.
`comments.json` can used to add comments to the types.
Finally, `knownWorkerTypes.json` determine which types are available in a WebWorker environment.

The format of each file can be inferred from their existing content.

The common steps to send a pull request are:

0. Open or refer to an issue in the [TypeScript repo](https://github.com/Microsoft/TypeScript).
1. Add missing elements to `inputfiles/addedTypes.json`, overriding elements to `inputfiles/overridingTypes.json`, or elements to remove to `inputfiles/removedTypes.json`.
2. Run the build script locally to obtain new `dom.generated.d.ts` and `webworker.generated.d.ts`.
3. Update the files in the `baselines` folder using the newly generated files
   under `generated` folder (`npm run baseline-accept`).

### When should a DOM API be included here?

If a DOM API is still a draft, it belongs on [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped#readme).
When it graduates past draft stage, it can be added here and removed from DefinitelyTyped.
A feature is considered a draft if:

#### W3C

A Working Draft should go on DefinitelyTyped.
A feature that is a Candidate Recommendation (or has passed that stage) should be added here.

#### WhatWG

A "Working Draft" ([example](https://whatwg.org/specs/web-controls/current-work/)) should go on DefinitelyTyped.
A "Living Standard" ([example](https://xhr.spec.whatwg.org/)) should be added here.


## Code Structure

- `src/index.ts`: handles the emitting of the `.d.ts` files.
- `src/test.ts`: verifies the output by comparing the `generated/` and `baseline/` contents.


## Input Files

- `browser.webidl.preprocessed.json`: a JSON file generated by Microsoft Edge. **Do not edit this file**.
    - Due to the different update schedules between Edge and TypeScript, this may not be the most up-to-date version of the spec.
- `mdn/apiDescriptions.json`: a JSON file generated by fetching API descriptions from [MDN](https://developer.mozilla.org/en-US/docs/Web/API). **Do not edit this file**.
- `addedTypes.json`: types that should exist in either browser or webworker but are missing from the Edge spec. The format of the file mimics that of `browser.webidl.preprocessed.json`.
- `overridingTypes.json`: types that are defined in the spec file but has a better or more up-to-date definitions in the json files.
- `removedTypes.json`: types that are defined in the spec file but should be removed.
- `comments.json`: comment strings to be embedded in the generated .js files.
