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
## Sample code: Create a new wallet

Duration: 4

The first thing we want to do is to create a new wallet.
Here is how:

```javascript
// import dash
const Dash = require("dash");

// client options
const clientOpts = {
 network: "testnet",  // as long as we set network to 'testnet' all operations
                       // performed with the client are only executed on evonet
                       // with eDash. eDash has no value
                       // as it is the testnetwork
  wallet: {
    mnemonic: null,  // this line tells the network
                       // that we want to create a new wallet
  }
};

// create a new client with the specified client options
const client = new Dash.Client(clientOpts);

// create a new wallet
async function createWallet() {
  try {
    // get the account
    const account = await client.wallet.getAccount();
    await account.isReady();

    // and read the new mnemonics from it
    const mnemonic = client.wallet.exportWallet();
    console.log('Mnemonic:', mnemonic);

    // also read out an unused address
    const address = account.getUnusedAddress();
    console.log('Unused address:', address.address);

  } catch (e) {
    console.error("Something went wrong:", e);
  } finally {
    client.disconnect();
  }
}

createWallet();

```

**When creating a new wallet, always make sure to save the mnemonic, otherwise you will not be able to access your wallet again.**

Save the file, then run `npm start` or `node src/index.js`.
Sample output:
![create wallet, sample output](assets/JS-Create-Wallet_Sample-Output.png)

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

Now, let's check the balance of the wallet:

```javascript
const Dash = require("dash");

const clientOpts = {
  network: "testnet",
  wallet: {
    mnemonic:
    // this time, we need to specify the mnemonic, replace with yours instead
      "avocado large rather uphold wife stereo glide knee bar relax neck pupil",
  },
};

async function checkAccoutBalance() {
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

checkAccoutBalance();
```

Run again with `npm start`. The output should be similar to:
![get balance, sample output](assets/JS-Get-Balance_Sample-Output.png)

<!-- ------------------------ -->
## Sample code: Send eDASH

Duration: 2

Now that you have a balance you can send some from one address to another.  Let's send 1 eDASH back to the wallet.  Update `main()` as follows:

```javascript
async function sendFunds() {
  const account = await client.wallet.getAccount();
  await account.isReady();

  try {
    // create a transaction, everything we need is the recipient and the amount in satoshis
    const transaction = account.createTransaction({
      recipient: "yNPbcFfabtNmmxKdGwhHomdYfVs6gikbPf", // Evonet faucet
      satoshis: 100000000, // 100000000 satoshis = 1 Dash
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

sendFunds();
```

You can now check the account balance again like in the previous step, to confirm that 1 Dash has been sent.

<!-- ------------------------ -->
## Congratulations, now play around

Duration: 0

If you were typing along you should have noticed that Typescript shows you autocompletion results for available properties and methods on classes and instances (e.g. `new Dash.Client()`, `client.getWalletAccount()`, etc).  With that you can play around with what the SDK has to offer.

For more examples see <https://dashplatform.readme.io/docs/tutorial-connecting-to-evonet> and <https://dashevo.github.io/DashJS/#/>.
