# Symbol QR Library

[![npm version](https://badge.fury.io/js/symbol-qr-library.svg)](https://badge.fury.io/js/symbol-qr-library)
[![Build Status](https://travis-ci.com/nemfoundation/symbol-qr-library.svg?branch=master)](https://travis-ci.com/nemfoundation/symbol-qr-library)
[![Slack](https://img.shields.io/badge/chat-on%20slack-green.svg)](https://nem2.slack.com/messages/CB0UU89GS//)

Library to generate QR codes for Symbol.

This is a PoC to validate the proposed [NIP 7 QR Library Standard Definition](https://github.com/nemtech/NIP/issues/3). When stable, the repository will be moved to the [nemtech](https://github.com/nemtech) organization.

**NOTE**: The author of this package cannot be held responsible for any loss of money or any malintentioned usage forms of this package. Please use this package with caution.

## Features

The software allows you to create the following QR types:

* **TransactionRequest**: QR to prepare transactions ready to be signed.
* **CreateAddContact**: QR to share the account address with others.
* **Mnemonic**: QR to generate account mnemonic backups.
* **CustomObject**: QR to export  a custom object.

## Requirements

- Node.js 12 LTS

## Installation

`npm install symbol-qr-library`


## Usage

### Generate QRCode for a Transaction Request

```typescript
import { QRCodeGenerator } from 'symbol-qr-library';

// (Optional) create transfer transaction (or read from network)
const transfer = TransferTransaction.create(
  Deadline.create(),
  Address.createFromPublicKey(
    'C5C55181284607954E56CD46DE85F4F3EF4CC713CC2B95000FA741998558D268',
    NetworkType.MIJIN_TEST
  ),
  [new Mosaic(new NamespaceId('cat.currency'), UInt64.fromUint(10000000))],
  PlainMessage.create('Welcome to NEM!'),
  NetworkType.MIJIN_TEST
);

// create QR Code base64
const request = QRCodeGenerator.createTransactionRequest(transfer);

// get base64 notation for <img> HTML attribute
const base64 = request.toBase64();
```

### Generate QRCode for a custom object

```typescript
import { QRCodeGenerator } from 'symbol-qr-library';

// define custom object to suit your application use case.
const object = {"obj": "test"};

// create QR Code base64
const request = QRCodeGenerator.createExportObject(object, NetworkType.TEST_NET);

// get base64 notation for <img> HTML attribute
const base64 = request.toBase64();

```

### Generate ContactQR code

```typescript
import {
    PublicAccount,
    NetworkType,
} from 'symbol-sdk';

const name = 'test-contact-1';
const account = PublicAccount.createFromPublicKey(
                'C5C55181284607954E56CD46DE85F4F3EF4CC713CC2B95000FA741998558D268',
                NetworkType.TEST_NET
            );

// create QR Code base64
  const request = QRCodeGenerator.createAddContact(name, account);

// get base64 notation for <img> HTML attribute
const base64 = request.toBase64();

```

### Generate QRCode for a Mnemonic data

```typescript
import {
    Account,
    NetworkType,
    Password,
} from 'symbol-sdk';
import { MnemonicPassPhrase } from 'symbol-hd-wallets';

// create a mnemonic and password.
const mnemonic = MnemonicPassPhrase.createRandom();

// create QR Code base64
const exportMnemonic = new MnemonicQR(mnemonic, 'password', NetworkType.MIJIN_TEST, 'no-chain-id');

// get base64 notation for <img> HTML attribute
const base64 = exportMnemonic.toBase64();

```

The produced Base64 encoded payload can be used to display the QR Code. An example of display can be done easily with HTML, as follows:

```html
<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO9TXL0Y4OHwAAAABJRU5ErkJggg==" alt="Transfer Transaction QR code" />
```

## Getting help

Use the following available resources to get help:

- [Symbol Documentation][docs]
- Join the community [slack group (#sig-client)][slack] 
- If you found a bug, [open a new issue][issues]

## Contributing

This project is developed and maintained by NEM Foundation.

Contributions are welcome and appreciated. 
Check [CONTRIBUTING](CONTRIBUTING.md) for information on how to contribute.

## License

Copyright 2019-present NEM

Licensed under the [Apache License 2.0](LICENSE)

[self]: https://github.com/nemfoundation/symbol-qr-library
[docs]: https://nemtech.github.io
[issues]: https://github.com/nemfoundation/symbol-qr-library/issues
[slack]: https://join.slack.com/t/nem2/shared_invite/enQtMzY4MDc2NTg0ODgyLWZmZWRiMjViYTVhZjEzOTA0MzUyMTA1NTA5OWQ0MWUzNTA4NjM5OTJhOGViOTBhNjkxYWVhMWRiZDRkOTE0YmU
