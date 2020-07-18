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

### What you'll learn

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

In VSCode, open the terminal with `Ctrl` + `` ` ``
*(Alternatively you can use the "normal" terminal from the previous steps)*

Then initialize the NPM project:

```sh
npm init
```

And install DashJS:

`npm install dash`

<!-- ------------------------ -->
## TypeScript setup

Duration: 2

If TypeScript is not installed already, install it:

`npm install --save-dev typescript`

*Alternatively, you can install globally by replacing `--save-dev` with `--global`*.

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

*Alternatively, you can install globally by replacing `--save-dev` with `--global`*.

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
## Sample code: basic template

Duration: 4

Copy the following example code into `app.ts`.  This is a basic template that we'll add to later:

```typescript
// import the Dash SDK:
import * as Dash from "dash";

// create a helper function to initializae a client
const initClient = (mnemonic = null) => {
  return new Dash.Client({
    network: "testnet", // all operations will only be on the testnet
    wallet: { mnemonic } // if mnemonic is null one will be created
  });
};

// declare a main function with business logic
const main = async () => {
  const client = await initClient()
  return "client initialzed sucessfully"
  // we'll add more here in later steps
}

// call the main function, recieve a value on success or error on failure
main()
  .then(val => console.log("Success :) \n", val, "\n"))
  .catch(err => console.log("Fail :( \n", err, "\n"))
  .finally(() => process.exit());
```

*Hint: To autofromat your TS code you can use one of the following shortcuts:*

* Windows: `Shift` + `Alt` + `F`
* Mac: `Shift` + `Option` + `F`
* Linux: `Ctrl` + `Shift` + `I` or `Shift` + `Alt` + `I`

If none of the above work, you can also try:
`Ctrl` + `K` and then `Ctrl` + `F`

Save the file, then run `tsc && node dist/app.js` or `npm start`.

<!-- ------------------------ -->
## Sample code: read account and network data

Duration: 3

Now, let's get a mnemonic, an unused address, and some network data, Update `main()` as follows:

```typescript
const main = async () => {
  const client = await initClient()
  const mnemonic = client.wallet.exportWallet();
  const account = await client.getWalletAccount();
  const unusedAddress = account.getUnusedAddress().address;
  const bestBlockHash = await client.getDAPIClient().getBestBlockHash();
  return { mnemonic, unusedAddress, bestBlockHash };
};

```

Make note of the `mnemonic` and `unusedAddress` from the console for the next step.

<!-- ------------------------ -->
## Sample code: request eDASH and check balance

Duration: 3

Before we can send eDash (Dash from "evonet" testnet) we need to have some. Request eDash from the [faucet Dash Core Group (DCG) runs](http://faucet.evonet.networks.dash.org/)

Then confirm that you have a balance on [DCG's block explorer](http://insight.evonet.networks.dash.org:3001/insight/)

You can also check the balance using DashJS.  Update `main()` as follows:

```typescript
const main = async () => {
  const client = await initClient('replace this string with your twelve word mnemonic string from previous step')
  const account = await client.getWalletAccount();
  const accountBalance = {
    unconfirmed: account.getUnconfirmedBalance(false),
    confirmed: account.getConfirmedBalance(false),
    total: account.getTotalBalance(false)
  };
  return { accountBalance };
};
```

<!-- ------------------------ -->
## Sample code: Send eDASH

Duration: 2

Now that you have a balance you can send some from one address to another.  Let's send 1 eDASH back to the wallet.  Update `main()` as follows:

```typescript
const main = async () => {
  const client = await initClient('replace this string with your twelve word mnemonic string from previous step')
  const account = await client.getWalletAccount();

  // create the transaction (this doesn't broadcast it)
  const transation = account.createTransaction({
    recipient: "yNPbcFfabtNmmxKdGwhHomdYfVs6gikbPf", // Evonet faucet
    satoshis: amountInDash * 100000000 // 1 eDASH
  });

  // broadcast the transaction
  const txid = await account.broadcastTransaction(transaction);
  return txid;
};
```

Now you can check the balance as shown in the previous step.

<!-- ------------------------ -->
## Congratulations, now play around

Duration: 0

If you were typing along you should have noticed that Typescript shows you autocompletion results for available properties and methods on classes and instnaces (e.g. `new Dash.Client()`, `client.getWalletAccount()`, etc).  With that you can play around with what the SDK has to offer.
