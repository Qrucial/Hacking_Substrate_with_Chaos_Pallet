# Hacking Substrate with Chaos Pallet

## Introduction
If you have followed QRUCIAL workshops at [Polkadot Hungary meetups](https://www.youtube.com/channel/UC0d-2y00kxiydKTWABgyg9g), you already know how to prepare a development environment for [Substrate](https://substrate.io/) and how to compile your own version. Security is crucial (pun intended) for all serious projects and in this tutorial we show you how to security test your own Substrate system using [Chaos Pallet](https://github.com/paritytech/pallet-chaos/]).
This blog post assumes you run Linux and understand the basic concepts of [blockchain](https://docs.substrate.io/v3/getting-started/glossary/#blockchain), [consensus](https://docs.substrate.io/v3/getting-started/glossary/#consensus), [pallets](https://docs.substrate.io/v3/getting-started/glossary/#pallet) and the [Substrate architecture](https://docs.substrate.io/v3/getting-started/architecture/). If you are not, you can read through these links and come back when you are ready for hacking!

## Threat model
Before getting engaged in any security assessment, it is important to have a look at the threat model. You can use these three questions in most scenarios.

What do we want to protect?
- The Substrate network we create (attack vectors are covered in ChaosScope's README).

What are the goals of the protection?
- Confidentiality (cryptographic keys are sufficiently secure and stored securely)
- Integrity (data is consistent, blockchain is immutable, block generation is sufficiently secure)
- Availability (users can access and use the network efficiently)
This is call a [CIA Triad](https://csrc.nist.gov/glossary/term/cia).

Who are the threat actors?
- Attackers sending requests from outside (eg. [Extrinsics](https://polkadot.js.org/docs/substrate/extrinsics/))

How much resources do we want to spend on the security assessment and protection?
- In this tutorial we spend 1 hour for the testing as we are focusing on the learning process. We spend 1 or more hours analyzing the findings and researching on protection.

If you'd like to learn more about Threat Modeling, you can use [NIST resources](https://csrc.nist.gov/glossary/term/threat_modeling) or contact us.
The 

## Testing with ChaosScope
*"Chaoscope makes Substrate Runtimes behave in ways that they're not supposed to..."* - What does it mean? ChaosScope uses [subxt](https://github.com/paritytech/subxt) to inject requests. A more accurate explanation is that we are injecting [Extrinsics](https://polkadot.js.org/docs/substrate/extrinsics/) aka transactions. We are attacking the Substrate system from the Pallet Chaos pallet like we are coming from an external point of view (now we consider nodes also external to each other, as each of them are individual parts of the whole network).

We are using a forked repository here as we are not aware what Linux system you are using. The original [ChaosScope repo](https://github.com/paritytech/chaoscope) does not include dependency check and might break the automated script, so [it was forked](https://github.com/smilingSix/chaoscope) and in this tutorial, we are using that fork. It should be working on [Kali](https://www.kali.org/), [Fedora](https://getfedora.org/), [Ubuntu](https://ubuntu.com/) and btw [Arch](https://archlinux.org/) systems.

The following commands are used from your choice of terminal to start the base Substrate system with Chaos Pallet included.

```sh
git clone https://github.com/smilingSix/chaoscope
cd chaosscope
bash chaosscope.sh
```
You should see the following if everything works as expected.

<img src="/media/chaos_scope_success.png">

What is happening now? You Substrated started to run and Chaos is being created. You can also check your local [listening network ports](https://www.tecmint.com/find-listening-ports-linux/) and filter the output using [grep](https://linux.die.net/man/1/grep) or [egrep](https://linux.die.net/man/1/egrep).
```sh
netstat -tunlp | egrep 'node|substrate'
```

The part "0.0.0.0:30333" means anyone can connect to the port 30333 (except if it is firewalled), to our Substrate node. The "127.0.0.1:9944" means these ports (you should also see port 9933 and 9615) are listening only locally, reachable only through direct access from your machine. If you want to learn more about networking on linux, you can read the ["Learn the networking basics"](https://www.redhat.com/sysadmin/sysadmin-essentials-networking-basics) guide.

Also, you can attach to the [screen](https://linuxize.com/post/how-to-use-linux-screen/) that was started by the script we just ran.
```sh
screen ls
screen -r <name>
```

Now, it is time for some visualization and looking into more what is happening with out Substrate system.

- Open [Polkadot JS app](https://polkadot.js.org/apps/)
- Swith to the local development network (click "Switch"):
<img src="/media/polkadot_js_switch_to_localhost_.png">

- You can skip this step if you are ok with the default development accounts:
    - Alternatively, install the Polkadot.js [extension](https://polkadot.js.org/extension/) for interacting with your running node and also to monitor what is happening.
    - After installion, create your first account. Here is the [official guide](https://wiki.polkadot.network/docs/learn-account-generation) if you need help with it.

- Now, try to make a transaction from Polkadot JS app and see if it works. 

- Meanwhile, if you wait long enough, you will see your Substrate going down:
<img src="/media/substrate_execution_stop_dos.png">

- Time to restart the whole thing and play with the attacks.
```sh
killall node-template # Stop all processes that are called node-template
substrate-node-chaos/target/release/node-template # Start the Substrate Node
```

- You can call the attack binaries separately.
```sh
./target/release/chaoscope -h                            # Use help on what you can do with this tool
./target/release/chaoscope overflow-adder -n 10000000    # Example

```

- Have fun hacking! 👾👾👾


## Evaluating chaos aka results and fixing issues
- What have you found? Which ways could you break your own running Substrate network?
    - Using the findings, now we can also write an exploit that targets Substrate nodes. How? Maybe in another blog post... ;)
- Note 1: You can monitoring logs from your Substrate node (log to file or see [stdout](https://linux.die.net/man/3/stdout))
- Note 2: For issues found in Rust, refer to the ["Rust security guidelines"](https://anssi-fr.github.io/rust-guide/01_introduction.html) article

## Conclusion
We have added a pallet to our Substrate system which helps us to uncover different kind of vulnerabilities (eg. DoS, overflows, economic attacks, etc). Tests were limited to a single node and it clearly shows why a standalone system is weak and why high capacity nodes are required to secure a blockchain system.
Also, we are sure if you have followed through this blog post, you also learned a lot. If you have feedback or questions, don't hesitate to send us a message on [QRUCIAL Twitter](https://twitter.com/qrucial_io).

