# DHIS2 SCP CLI

This NPM package helps you to create NPM packages with React components that can be used to build DHIS2 apps or other components.

This package provides a command line interface `dhis2-scp-cli` with various commands.

* `dhis2-scp-cli verify`: This command will check the quality of your npm package.

The command will do the following:

* Verify that the package's `package.json` has the `dhis2-component-search` keyword (see section 1.1).
* Verify that the package's `package.json` has the `dhis2ComponentSearch` property with correct values and structure (see section 1.3).
* Verify that the package passes an `npm audit` check.
* Verify that the package passes an `eslint` check.

# Package verification guide

## 1 Verification prerequisites

### 1.1 Keyword

Your `package.json` file must include `dhis2-component-search` keyword as follows:

```json
{
  "keyword": [
      "dhis2-component-search"
  ]
}
```

### 1.2 Repository

Currently we only support packages hosted on Github.
Your `package.json` file must include `repository` property, that includes key/value pairs for repository type and url. 
Inside your `package.json`it would look like this:

```json
{
   "repository": {
        "type" : "git",
        "url" : "https://github.com/npm/cli.git"
    }
}
```
The URL should be a publicly available and you must specify the repository type and url. Note that you cannot use shortcut syntax, e.g. `"repository": "github:user/repo"`.


### 1.3 The `dhis2ComponentSearch` property

The `dhis2ComponentSearch` property of a `package.json` file includes the information about the package that is relevant to The Shared Component Platform and its search functionality.

The `dhis2ComponentSearch` property must include key/value pairs for framework (using `language` key, we currently support `react` and `angular`), and components. The `component` property, in turn, takes an array of component objects. Each component object must include following information defined as key/value pairs:

__Required__:
* The `name` property contains a name of the exported component
* The `export` property contains an export of the exported component
* The `description` property contains description of the exported component

__Optional__:

* The `dhis2Version` optional property contains the DHIS2 versions supported by the exported component, the versions must be specified as an array of strings.

Inside your `package.json`, the `dhis2ComponentSearch` property may look something like this:

```json
{
   "dhis2ComponentSearch": {
    "language": "react",
    "components": [
      {
        "name": "Organizational Unit Tree",
        "export": "OrgUnitTree",
        "description": "A simple OrgUnit Tree",
        "dhis2Version": [
          "32.0.0",
          "32.1.0",
          "33.0.0"
        ]
      }
    ]
   }
}
```

### 1.4 Components as commonJS modules

Our verification process requires your components to be distributed as [commonJS modules](https://en.wikipedia.org/wiki/CommonJS). The command `npm install` should result in a valid commonJS module.
One good way to achieve this is with the help of [create-react-library](https://www.npmjs.com/package/create-react-library) CLI, as it bundles `commonjs` and `es` module formats.

### 1.5 NPM and Github

Your package must be published on [NPM](https://www.npmjs.com) and have a public [Github](https://www.github.com) repository. Since we verify a specific version of the package, a git tag with this version should be added. When tagging releases in a version control system, the tag for a version must be `vX.Y.Z` e.g. `v1.0.2`.

## 2 Pre-verification

You can use [dhis2-scp cli](https://github.com/dhis2designlab/scp-cli) to verify your package locally before you submit your package for verification. It provides a command
```properties
dhis2-scp-cli verify
```
that runs all the checks included in the verification.

## 3 Verification process

When all the prerequisites are met, you may proceed with the verification. To submit your package for verification you would need to modify [`list.csv`](https://github.com/dhis2designlab/scp-whitelist/blob/main/list.csv) file by adding a new line containing your npm package `identifier`and its `version` separated by a comma, e.g. `lodash,4.17.14`. Since you do not have a write access to the repository, a change in this file will
write it to a new branch in your fork, so you can make a pull request. Fill in a title and description and create your pull request, which in turn, will trigger the verification workflow on your package.

The verification workflow:

* Checks for verification prerequisites defined in section 1
* Lints the code
* Runs npm audit

