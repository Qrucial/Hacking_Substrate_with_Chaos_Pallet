# Hacking Substrate with Chaos Pallet

## Introduction
If you have followed QRUCIAL workshops at [Polkadot Hungary meetups](https://www.youtube.com/channel/UC0d-2y00kxiydKTWABgyg9g), you already know how to prepare a development environment for [Substrate](https://substrate.io/) and how to compile your own version. Security is crucial (pun intended) for all serious projects and in this tutorial we show you how to security test your own Substrate system using [Chaos Pallet](https://github.com/paritytech/pallet-chaos/]).


## Threat model
Before getting engaged in any security assessment, it is important to have a look at the threat model. You can use these three questions in most scenarios.

What do we want to protect?
- The Substrate network we create.

What are the goals of the protection?
- Confidentiality (cryptographic keys are sufficiently secure and stored securely)
- Integrity (data is consistent, blockchain is immutable, block generation is sufficiently secure)
- Availability (users can access and use the network efficiently)
This is call a [CIA Triad](https://csrc.nist.gov/glossary/term/cia).

Who are the threat actors?
- Attackers sending requests from outside (eg. [Extrinsics](https://polkadot.js.org/docs/substrate/extrinsics/))

How much resources do we want to spend on the security assessment and protection?
- In this tutorial we spend 1 hour for the testing as we are focusing on the learning process.
- We spend 1 hour analyzing the findings.

If you'd like to learn more about Threat Modeling, you can use [NIST resources](https://csrc.nist.gov/glossary/term/threat_modeling) or contact us.


## Testing with ChaosScope
*"Chaoscope makes Substrate Runtimes behave in ways that they're not supposed to..."* - What does it mean? ChaosScope uses [subxt](https://github.com/paritytech/subxt) to inject requests. A more accurate explanation is that we are injecting [Extrinsics](https://polkadot.js.org/docs/substrate/extrinsics/) aka transactions. We are attacking the Substrate system from the Pallet Chaos pallet like we are coming from an external point of view (now we consider nodes also external to each other, as each of them are individual parts of the whole network).

https://github.com/smilingSix/chaoscope

## Using Chaos Pallet more manually and compiling Substrate
### simple way
```sh
git clone https://github.com/paritytech/pallet-chaos
cargo build --release
#Use frontend to add pallet
```

## Monitoring and evaluating chaos
- go to frontend and extrinsics
- monitoring chain states

## Conclusion
We have added a pallet to our Substrate system which helps us to uncover different kind of vulnerabilities (eg. DoS, overflows, economic attacks, etc).
Also, we are sure if you have followed through this blog post, you also learned a lot. If you have questions, don't hesitate to send us a message on Twitter: qrucial_io
