# PercentURL decoder regression fixtures

Small, dependency-free Base64 and Base32 checks extracted from the live PercentURL tools. The scripts and decoder cores are unchanged from site commit `7abebde9edc115b975c7eb262420644e2da759b2` (October 25, 2026).

## Run

Install Python 3 and Node.js, then run these commands from this folder. No package installation or network requests are required by the scripts.

```sh
python base64-decode-regression.py base64-decoder-core.js
node base32-decode-regression.cjs
```

## What is checked

- **Base64:** nine inputs compared across the page decoder, Python `base64.b64decode(validate=True)`, and Node `Buffer.from(input, 'base64')`. Cases include non-zero pad bits, missing padding, URL-safe characters, binary/non-UTF-8 bytes, excess padding, impossible lengths, and whitespace. SHA-256 is computed on decoded bytes before text interpretation. This script prints a comparison matrix; it is not an assertion-based pass/fail suite.
- **Base32:** thirteen fixtures assert decoded bytes or rejection. They cover the RFC 4648 `f` through `foobar` vectors, lowercase/unpadded input, binary byte `ff`, non-zero pad bits, impossible lengths, wrong padding, whitespace, and a character outside the standard alphabet. A separate assertion checks Base32hex `CPNMU===` to `foo`.

Expected Base32 output:

```text
13 fixtures passed; Base32hex foo passed
```

`base64-decode-results.json` and `base32-decode-results.json` are dated historical run records, not automatically refreshed results. Rerun the scripts to see your installed runtimes' behavior. The Base32 record also describes the original 257-array / 1,028-combination Python-encoder crosscheck; the small replay script included here runs the thirteen fixtures and Base32hex example, not that larger crosscheck.

## Decoder policies and scope

These are this project's compatibility choices, not a claim that every decoder must behave identically. Base32 accepts lowercase and omitted terminal padding but rejects whitespace, invalid lengths/alphabet characters, incorrect supplied padding, and non-zero trailing pad bits. Base32hex must be selected explicitly; Crockford Base32 is not implemented.

The cores return bytes. Text interpretation is a separate step; decoded bytes need not be valid UTF-8. The browser UI in `base32-decoder.js` is guarded when running under Node. Neither script verifies signatures, decrypts data, or validates a whole protocol. Fixed fixtures are regression evidence, not proof of correctness for every input.

Format reference: [RFC 4648](https://www.rfc-editor.org/rfc/rfc4648.html). Code is available under the included MIT license.

Live tools and explanations: [PercentURL](https://percenturl.com/)
