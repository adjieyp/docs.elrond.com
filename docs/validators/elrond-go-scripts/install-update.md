---
id: install-update
title: Installing a Validator Node
---

## **Install your node(s)**

After preparing the user permissions, the script configurations, and the keys, the actual node installation can begin. The Validator script is a multi-purpose tool for managing your node, it is accessible to Devnet and Testnet Network.

- *Note: This process installs a validator node for all Elrond networks (mainnet, devnet, and testnet).*

Following these few steps, we will work on installing the Elrond Network validator node to get it up and running on your local machine.

 For installation use `1 - install`

```bash
 ~/elrond-go-scripts/script.sh
 1) install
 2) observing_squad
 3) upgrade
 4) upgrade_squad
 5) upgrade_proxy
 6) remove_db
 7) start
 8) stop
 9) cleanup
 10) github_pull
 11) add_nodes
 12) get_logs
 13) benchmark
 14) quit
 Please select an action:1
```

- When asked, indicate the number of nodes you want to run, i.e. `1`
- When asked, indicate the name of your validator, i.e. `Valar`
- Quit the menu without starting (we need keys first) by using `14 - quit`

### **Prepare your keys**

Create a new folder "VALIDATOR_KEYS" to serve as a local backup when updating:

```bash
cd ~
mkdir -p ~/VALIDATOR_KEYS
    
```

Generate a certificate file containing your Validator key by running the `keygenerator` :

 ```bash
./elrond-utils/keygenerator
    
```

Copy the generated `validatorKey.pem` file to the `config` folder of your node(s), and repeat for each node.

```bash
    cp validatorKey.pem ~/elrond-nodes/node-0/config/
    
```

:::tip
Each node needs its unique `validatorKey.pem` file
:::

Then copy the `validatorKey.pem` file - in ZIP form - to the `$HOME/VALIDATOR_KEYS/` folder . This is important for your node to be able to restart correctly after an upgrade.

```bash
zip node-0.zip validatorKey.pem
mv node-0.zip $HOME/VALIDATOR_KEYS/
    
```

Repeat the above process for all your “n” nodes. When complete, please refer to our Key management section for instructions about how to properly backup and protect your keys.

### **Start the node(s)**

```bash
~/elrond-go-scripts/script.sh start
```

### **Start the node visual interface**

Once the node has started, you can check its progress, using the `TermUI` interface. Navigate to your `$HOME/elrond-utils` directory and start the `TermUI`, one for each of your nodes:

```bash
cd $HOME/elrond-utils
./termui -address localhost:8080
```

:::tip

 Your first node is called `node-0` and it is a REST API that will run on port `8080` by default. The next node is `node-1`on port `8081`, and so on.
:::

## **Update your node(s)**

Upgrade your node by running the script and selecting either of these options:

- `10 - github_pull` downloads the latest version of the scripts
- `3 - upgrade`
- `7 - start`
- `14 - quit`

```bash
~/elrond-go-scripts/script.sh
```

These are the basic steps. Please carefully read the on-screen instructions, refer to the scripts [readme file](https://github.com/ElrondNetwork/elrond-go-scripts/blob/master/README.md). You can also ask any questions in the Elrond [Validators chat](https://t.me/ElrondValidators)

## **Mandatory: Backup your keys**

Your private keys are needed to run your node. Losing them means losing control of your node. A 3rd party gaining access to them could result in loss of funds.

Find them in `$HOME/elrond-nodes/node-0/config` [be mindful of your “`n`” nodes]

:::important
Create a safe backup for them on storage outside of the server running your node(s).
:::

## **Migration from old scripts**

Before the release of `elrond-go-scripts`, there were `elrond-go-scripts-testnet` and `elrond-go-scripts-devnet` for setting up nodes
on testnet of devnet. If one wants to migrate from the old scripts to the new ones, these are the steps to be followed:

- the migration is possible while preventing the keys, DB, logs and the current node installation, in general
- just clone the `elrond-go-scripts` repo near the old one (`elrond-go-scripts-testnet`/`elrond-go-scripts-testnet`), configure it as stated and
  call the `migrate` operation on the scripts:
```
cd ~/elrond-go-scripts
./script.sh migrate
```

this will tell the scripts the current environment on which the previous installation was done. Be careful as to
not mix the previous installation network with the new one. This might lead to unpredictable results

## **Choosing a custom configuration tag or branch**

:::warning
This option should be only used when debugging or testing in advance of a pre-release tag.
Use this on your own risk!
:::

The power of the scripts set has been leveraged with a new addition: the possibility to tell the scripts a specified tag
or branch (not recommended using a branch due to the fact that an unsigned commit might bring malicious code or configs)

To accomplish this, edit the variables.cfg file
```
cd ~/elrond-go-scripts/config
nano variables.cfg
```
locate the `OVERRIDE_CONFIGVER` option and input a value there, something like `tags/T1.3.14.0`.
The `tags/` prefix will tell the scripts to use the tag and not search a branch called `T1.3.14.0`.
Call the `upgrade` command on the scripts to install the desired configuration version.

Resetting the value to `""` will make the scripts to use the released version.

:::warning
The `OVERRIDE_CONFIGVER` is not backed up when calling `github_pull` operation.
:::
