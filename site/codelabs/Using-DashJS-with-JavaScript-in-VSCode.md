---
summary: Using DashJS with JavaScript in VSCode
id: Using-DashJS-with-JavaScript-in-VSCode
categories: JavaScript
tags: medium
status: Published
authors: Rion, Schlomon
feedback link: https://github.com/Schlomon/Dash-Codelab-Tutorials/issues

---

# How to use DashJS with JavaScript in VSCode

<!-- ------------------------ -->
## Overview

Duration: 1

### What you'll learn

* How to set up a project which is able to interact with the Dash network
* How to create and manage a Wallet (e.g. check a balance, send Dash, etc)

<!-- ------------------------ -->
## NPM setup

Duration: 2

Create a new project folder:

```sh
mkdir dashJS_example
cd dashJS_example/
```

You can open it in VSCode now:

```sh
code .
```

In VSCode, open the terminal with `Ctrl` + `` ` ``
*(Alternatively you can use the "normal" terminal from the previous steps)*

Then initialize the NPM project:

```sh
npm init
```

Create a `.js` file for later use:

```sh
mkdir src
touch src/index.js
```

And install DashJS:

`npm install dash`

<!-- ------------------------ -->
## Optional setup

Duration: 3
There are a few options for additional setup. You can:

### Install a linter

To install a linter

1. Go to the `Extensions` tab (`Ctrl` + `Shift` + `X`) and searching for `ESLint`.
2. Click on `Install`
3. In the console, type `npm install eslint`

*For more information on how to setup a convenient JS workspace in VSCode see <https://code.visualstudio.com/Docs/languages/javascript>*

### Add a start script

1. Open the file `package.json` in VSCode
2. Insert `"start": "node src/index.js",` **before** the line beginning with `"test":`.
3. Save and close the file.

Now You can simply type `npm start` and it will automatically run the code in `index.js`.

<!-- ------------------------ -->
## Check if setup completed correctly

Duration: 1

You should now have the following file structure:

```text
dashJS_example
|-- node_modules
|   |-- (all the node modules)
|-- src
|   |-- index.js
|-- package.json
|-- package-lock.json
```

To test the setup place `console.log("hello")` inside `src/index.js`.
Then in the console type `npm start`
If everything worked you should get an output similar to:

```text
> dashjs_example@1.0.0 start ~/dashJS_example
> node src/index.js

hello
```

<!-- ------------------------ -->
## Sample code: basic template

Duration: 4

Copy the following example code into `src/index.js`.  This is a basic template that we'll add to later:

```javascript
// some js
```

*Hint: To autoformat your JS code you can use one of the following shortcuts:*

* Windows: `Shift` + `Alt` + `F`
* Mac: `Shift` + `Option` + `F`
* Linux: `Ctrl` + `Shift` + `I` or `Shift` + `Alt` + `I`

If none of the above work, you can also try:
`Ctrl` + `K` and then `Ctrl` + `F`

When you do this the first time you might have to choose a code formatter. Just click on `configure` on the popup window and choose one of the codeformatters.

Save the file, then run `npm start` or `node src/index.js`.

<!-- ------------------------ -->
## Sample code: read account and network data

Duration: 3

Now, let's get a mnemonic, an unused address, and some network data, Update `main()` as follows:

```javascript
// more js code
```

Make note of the `mnemonic` and `unusedAddress` from the console for the next step.

<!-- ------------------------ -->
## Sample code: request eDASH and check balance

Duration: 3

Before we can send eDash (Dash from "evonet" testnet) we need to have some. Request eDash from the [faucet Dash Core Group (DCG) runs](http://faucet.evonet.networks.dash.org/)

Then confirm that you have a balance on [DCG's block explorer](http://insight.evonet.networks.dash.org:3001/insight/)

You can also check the balance using DashJS.  Update `main()` as follows:

```javascript
// and more
```

<!-- ------------------------ -->
## Sample code: Send eDASH

Duration: 2

Now that you have a balance you can send some from one address to another.  Let's send 1 eDASH back to the wallet.  Update `main()` as follows:

```javascript
// bla bla
```

Now you can check the balance as shown in the previous step.

<!-- ------------------------ -->
## Congratulations, now play around

Duration: 0

If you were typing along you should have noticed that Typescript shows you autocompletion results for available properties and methods on classes and instances (e.g. `new Dash.Client()`, `client.getWalletAccount()`, etc).  With that you can play around with what the SDK has to offer.

For a more technical and in-depth documentation see <https://dashplatform.readme.io/docs>. Expecially under the Tutorials section.
