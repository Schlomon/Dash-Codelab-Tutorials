---
summary: Using DashJS with TypeScript in VSCode
id: Using-DashJS-with-TypeScript-in-VSCode
categories: Typescript
tags: medium
status: Published
authors: Rion, Schlomon
feedback link: https://github.com/Schlomon/DashJS-TS-Tutorial/issues

---


# How to use DashJS with TypeScript in VSCode

<!-- ------------------------ -->
## Overview

Duration: 1

### What You'll learn

* How to set up a project which is able to interact with the Dash network
* How to create and manage a Wallet (e.g. check a balance, send Dash, etc)

<!-- ------------------------ -->
## NPM setup

Duration: 2

Create a new project folder:

```sh
mkdir dashTS_example
cd dashTS_example
```

You can open it in VSCode now:

```sh
code .
```

In VSCode, open the terminal with `Ctrl` + `
*(Alternatively you can use the "normal" terminal from the previous steps)*

Then initialize the NPM project:

```sh
npm init
```

And install DashJS:

`npm install --save-dev dash`

You can also install dash and all the following npm packages globally, by replacing the option `--save-dev` with `--global`.

<!-- ------------------------ -->
## TypeScript setup

Duration: 2

If TypeScript is not installed already, install it:

`npm install --save-dev typescript`

Also create an output folder for later:

```sh
mkdir dist
```

Then run the following to complete the setup:

```sh
npx tsc --init
```

You can also create a file called `app.ts` for later use.

<!-- ------------------------ -->
## Optional setup

Duration: 3
There are a few options for additional setup. You can:

### Install a linter

* using the VSCode plugin `TSLint` (*recommended*), or
* via NPM: `npm install --save-dev typescript`

### Automate the compilation of TypeScript

1. Open the file `tsconfig.json` in VSCode
2. Uncomment the line beginning with `"outDir"`
3. Replace `"./"` with `"./dist"`, then save and close the file.
4. Open the file `package.json` in VSCode
5. Insert `"start": "tsc && node dist/app.js",` **before** the line beginning with `"test"`.
6. Save and close the file.

Now You can simply type `npm start` and it will automatically compile and run your code from `app.ts`

<!-- ------------------------ -->
## Check if setup completed correctly

Duration: 1

You should now have the following file structure:

```sh
dashTS_example
|-- dist
|-- node_modules
|   |-- (all the node modules)
|-- app.ts
|-- package-lock.json
|-- package.json
|-- tsconfig.json
```

To test the setup place `console.log("hello")` inside `app.ts`.
Then in the console type `npm start`
If everything worked you should get an output similar to:

```text
> dashts_example@1.0.0 start /dashTS_example
> tsc && node dist/app.js

hello
```

<!-- ------------------------ -->
## Sample Code: Creating a new Wallet

Duration: 4

You can write all of the following example code into `app.ts`.

First, import the Dash SDK:

```typescript
import * as Dash from "dash";
```

Then we need a new client. It's pretty simple to create one, but we need to do it a lot, which means it's best practice to put it into a function:

```typescript
const initClient = (mnemonic = null) => {
  return new Dash.Client({
    network: "testnet", // As long as this line is kept, all operations will only be on the testnet
    wallet: { mnemonic } // If mnemonic is null, Wallet-lib will create new mnemonic
  });
};
```

Now, in order to create a wallet and retrieve some useful data from it, You simply do:

```typescript
// get and print mnemonic, unused adresse, etc. here
```

Hint: To autofromat your TS code you can use one of the following shortcuts:

* Windows: `Shift` + `Alt` + `F`
* Mac: `Shift` + `Option` + `F`
* Linux: `Ctrl` + `Shift` + `I` or `Shift` + `Alt` + `I`

If non of the above work, you can also try:
`Ctrl` + `K` and then `Ctrl` + `F`

<!-- ------------------------ -->
## Sample Code: Reading the account balance

Duration: 3

Reading the account balance is really easy too:

```typescript
const client = initClient(mnemonic);  // calling the method from before
// read and print account balance here, preferably in a function cuz of the next two steps
```

<!-- ------------------------ -->
## Sample Code: Requesting some tDash

Duration: 3

Before we can spent some Dash we need to have some. In the testnet you can request some tDash (test Dash):

```typescript
// request tDash, probably print account balance again
```

<!-- ------------------------ -->
## Sample Code: Sending Dash

Duration: 2

```typescript
// send Dash, probably print account balance again
```
