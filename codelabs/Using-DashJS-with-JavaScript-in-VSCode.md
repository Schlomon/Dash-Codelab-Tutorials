---
summary: Using DashJS with JavaScript in VSCode
id: Using-DashJS-with-JavaScript-in-VSCode
categories: javascript
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

* How to set up a JavaScript project which is able to interact with the Dash network.
* How to create a Dash wallet.
* How to use the evonet faucet.
* How to check the balance of your wallet.
* How to send funds.

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

If prompted, hit `Enter` a couple of times.

Create a `.js` file for later use:

```sh
mkdir src
touch src/index.js
```

Make sure you are still in the root directory of the project and install DashJS:

```sh
npm install dash
```

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

To test the setup place `console.log("hello");` inside `src/index.js`.
Then in the console type `npm start`.
If everything worked you should get an output similar to:

```text
> dashjs_example@1.0.0 start ~/dashJS_example
> node src/index.js

hello
```

Delete the content of `index.js` again.

<!-- ------------------------ -->
## Sample code: Create a new wallet

Duration: 4

The first thing we want to do is to create a new wallet.
Here is how:

```javascript
// import dash
const Dash = require("dash");

// define client options
const clientOpts = {
 network: "evonet",  // as long as network is set to 'evonet', all operations
                      // performed with the client are only executed on evonet
                      // with eDash. eDash has no value.
  wallet: {
    mnemonic: null,   // this line tells the network
                      // that we want to create a new wallet
  }
};

// create a new client with the specified client options
const client = new Dash.Client(clientOpts);

// create a wallet creation function
async function createWallet() {
  try {
    // get the account
    const account = await client.wallet.getAccount();
    await account.isReady();

    // read the mnemonic from the client
    const mnemonic = client.wallet.exportWallet();
    console.log('Mnemonic:', mnemonic);

    // and get an unused address from the account
    const address = account.getUnusedAddress();
    console.log('Unused address:', address.address);

  } catch (e) {
    console.error("Something went wrong:", e);
  } finally {
    // always make sure to disconnect the client
    client.disconnect();
  }
}

createWallet();

```

**When creating a new wallet, always make sure to save the mnemonic, otherwise you will not be able to access your wallet again.**

Save the file, then run `npm start` or `node src/index.js`.
Sample output:

```text
> dashjs_example@1.0.0 start ~/dashJS_example
> node src/index.js

Mnemonic: replace this string with your twelve word mnemonic string from previous step
Unused address: yX3ohM9u8q4PXmv2QVmghXrpx8npKSMvAU
```

It is recommended to get a new unused address for every transaction.

*Hint: To auto format your JS code you can use one of the following shortcuts:*

* Windows: `Shift` + `Alt` + `F`
* Mac: `Shift` + `Option` + `F`
* Linux: `Ctrl` + `Shift` + `I` or `Shift` + `Alt` + `I`

If none of the above work, you can also try:
`Ctrl` + `K` and then `Ctrl` + `F`

When you do this the first time you might have to choose a code formatter. Just click on `configure` on the popup window and choose one of the codeformatters.

<!-- ------------------------ -->
## Sample code: request eDASH and check balance

Duration: 3

Before we can send eDash we need to have some. Request eDash from the [Dash Faucet](http://faucet.evonet.networks.dash.org/).
Just paste the unused address from the previous step in the textbox and hit `Get coins`.

![get coins](assets/JS-Get-Coins_Sample.png)

Now, let's check the balance of the wallet.

Replace the client options from before with these:

```javascript
const clientOpts = {
  network: "evonet",
  wallet: {
    mnemonic:
    // this time, we need to specify the mnemonic, replace with yours instead
      "replace this string with your twelve word mnemonic string from previous step",
  },
};
```

And add a new function and call it:

```javascript
async function checkAccountBalance() {
  const account = await client.wallet.getAccount();
  await account.isReady();

  try {
    // read the balance from the account
    const balance = {
      confirmed: account.getConfirmedBalance(false/**/),
      unconfirmed: account.getUnconfirmedBalance(false),
      total: account.getTotalBalance(false),
    };
    console.log(balance);
  } catch (e) {
    console.error("Something went wrong:", e);
  } finally {
    client.disconnect();
  }
}

checkAccountBalance();
```

Comment out the last line from the previous step `// createWallet();`.

Then run the script again with `npm start`. The output should be similar to:

```text
> jstest@1.0.0 start ~/dashJS_example
> node src/index.js

{ confirmed: 37.5931, unconfirmed: 0, total: 37.5931 }
```

<!-- ------------------------ -->
## Sample code: Send eDASH

Duration: 2

Now that you have a balance, you can send some from one wallet to another. Let's send 1 eDASH back to the faucet's wallet.  Add the following function:

```javascript
async function sendFunds(amountInDash) {
  const account = await client.wallet.getAccount();
  await account.isReady();

  try {
    // create a transaction, everything we need is the recipient's address and the amount to be sent in satoshis
    const transaction = account.createTransaction({
      recipient: "yNPbcFfabtNmmxKdGwhHomdYfVs6gikbPf", // Evonet faucet
      satoshis: Math.round(amountInDash * 100000000), // 100000000 satoshis = 1 Dash
    });

    // broadcast the transaction and get the transaction id
    const transactionId = await account.broadcastTransaction(transaction);  // you can send the id to the recipient so he can confirm that you have broadcasted the transaction
    console.log("Transaction broadcast!\nTransaction ID:", transactionId);
  } catch (e) {
    console.error("Something went wrong:", e);
  } finally {
    client.disconnect();
  }
}

sendFunds(37.5931);  // replace this number with however many DASH you have in your wallet to send everything back to the faucet.
```

Again, comment out the last line from the previous step `// checkAccountBalance();`
and run with `npm start`.

The output should be similar to:

```text
> dashjs_example@1.0.0 start ~/dashJS_example
> node src/index.js

Transaction broadcast!
Transaction ID: 8699ef50e08b6c3a318beaf85928e66bdbf67857a0c560050a3655e68f265c18
```

You can now check the account balance again like in the previous step, to confirm that 'however many Dash you had' have been sent.

<!-- ------------------------ -->
## Congratulations, now play around

Duration: 1

If you were typing along you should have noticed that VSCode shows you autocompletion results for available properties and methods on classes and instances (e.g. `new Dash.Client()`, `client.getWalletAccount()`, etc).  With that you can play around with what the SDK has to offer.

The full DashJS documentation can be found under <https://dashevo.github.io/DashJS/#/>.
For more code examples on e.g. Identities, Usernames, Datacontracts, etc. please visit <https://dashplatform.readme.io/docs/tutorial-register-an-identity>.

<!-- ------------------------ -->
## index.js

Duration: 0

Just in case you got stuck, here is the full `index.js` file how it should look like after you have followed this tutorial:

```javascript
// import dash
const Dash = require("dash");

// define client options
const clientOpts = {
  network: "evonet",
  wallet: {
    mnemonic:
      // this time, we need to specify the mnemonic, replace with yours instead
      "replace this string with your twelve word mnemonic string from previous step",
  },
};

// create a new client with the specified client options
const client = new Dash.Client(clientOpts);

async function createWallet() {
  try {
    // get the account
    const account = await client.wallet.getAccount();
    await account.isReady();

    // read the mnemonic from the client
    const mnemonic = client.wallet.exportWallet();
    console.log("Mnemonic:", mnemonic);

    // and get an unused address from the account
    const address = account.getUnusedAddress();
    console.log("Unused address:", address.address);
  } catch (e) {
    console.error("Something went wrong:", e);
  } finally {
    // always make sure to disconnect the client
    client.disconnect();
  }
}

async function checkAccountBalance() {
  const account = await client.wallet.getAccount();
  await account.isReady();

  try {
    const balance = {
      confirmed: account.getConfirmedBalance(false),
      unconfirmed: account.getUnconfirmedBalance(false),
      total: account.getTotalBalance(false),
    };
    console.log(balance);
  } catch (e) {
    console.error("Something went wrong:", e);
  } finally {
    client.disconnect();
  }
}

async function sendFunds() {
  const account = await client.wallet.getAccount();
  await account.isReady();

  try {
    // create a transaction, everything we need is the recipient and the amount in satoshis
    const transaction = account.createTransaction({
      recipient: "yNPbcFfabtNmmxKdGwhHomdYfVs6gikbPf", // Evonet faucet
      satoshis: 235.6932 * 100000000, // 100000000 satoshis = 1 Dash
    });

    // broadcast the transaction and get the transaction id
    const transactionId = await account.broadcastTransaction(transaction);
    console.log("Transaction broadcast!\nTransaction ID:", transactionId);
  } catch (e) {
    console.error("Something went wrong:", e);
  } finally {
    client.disconnect();
  }
}

// createWallet();

// checkAccountBalance();

// sendFunds();
```

<!-- ------------------------ -->
## package.json

Duration: 0

And for completion here is the package.json file:

```json
{
  "name": "dashjs_example",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "node src/index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "MIT",
  "dependencies": {
    "dash": "^3.14.1",
    "eslint": "^7.6.0"
  }
}
```
