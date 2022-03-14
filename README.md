# Hacking Substrate with Pallet Chaos

## Introduction
If you have followed QRUCIAL workshops at Polkadot Hungary meetups, you already know how to prepare a development environment for Substrate and how to compile your own version. Security is crucial (pun intended) for all serious projects and in this tutorial we show you how to test your own Substrate system using Pallet Chaos.

# What is Pallet Chaos?


# Threat model
Threat actors:
- Malicious nodes
- Malicious accounts
- Attacker sending requests from outside
etc tba

### ChaosScope
"Chaoscope makes Substrate Runtimes behave in ways that they're not supposed to..." - What does it mean? ChaosScope uses subxt to inject requests/transactions. The more accurate term to use here is "Extrinsics" which basically means injection of data to the Substrate system from an external point of view.

https://github.com/smilingSix/chaoscope

## Using Chaos Pallet more manually and compiling Substrate
### simple way
```sh
git clone https://github.com/paritytech/pallet-chaos
cargo build --release
#Use frontend to add pallet
```

## Monitoring and evaluating chaos
- go to frontend and extinsics
- monitoring chain states

## Conclusion
We have added a pallet to our Substrate system which helps us to uncover different kind of vulnerabilities (eg. DoS, overflows, economic attacks, etc).
Also, we are sure if you have followed through this blog post, you also learned a lot. If you have questions, don't hesitate to send us a message on Twitter: qrucial_io
