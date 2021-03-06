<!--
⚠️ DO NOT EDIT THIS FILE ⚠️

This file is generated.
  - To edit the descriptions see docs/README.md.hbs
  - To edit the API section see the jsdoc in the described source files.
-->

<h1 align="center">
  <br/>
  <a href='https://github.com/ConsenSys/web3studio-soy'><img
      width='250px'
      alt=''
      src="https://user-images.githubusercontent.com/5770007/50840308-2f093000-1330-11e9-996a-2e61a8b7fd9a.png" /></a>
  <br/>
</h1>

<h4 align="center">
  ENS+IPFS ❤ DevOps - Static Websites on the Distributed Web
</h4>

<p align="center">
  <a href="#usage">Usage</a> ∙
  <a href="#packages">Packages</a> ∙
  <a href="#api">API</a> ∙
  <a href="#contributing">Contributing</a> ∙
  <a href="#license">License</a>
</p>

Soy is a collection of smart contracts and tools to enable you to build your site
on the distributed web. By virtue of using [ENS](https://ens.domains/) and
[IPFS](https://ipfs.io/) your content will be quickly accessible all over the world without
having to set up or manage any infrastructure.

Already have an ENS resolver? Add `.soy` to the end to see it in your browser! Check
out [web3studio.eth.soy][web3studio.eth.soy]

<br/>

## Usage

### Install

```bash
# Yarn
$ yarn add --dev soy-core

# NPM
$ npm install --save-dev soy-core
```

### Configure

Create a new soy instance and give it any Web3 provider.

```js
const Soy = require('soy-core');
const HDWalletProvider = require('truffle-hdwallet-provider');

const mnemonic = process.env.WALLET_MNEMONIC;
const infuraApiKey = process.env.INFURA_API_KEY;
const infuraNetwork = process.env.INFURA_NETWORK;
const provider = new HDWalletProvider(
  mnemonic,
  `https://${infuraNetwork}.infura.io/v3/${infuraApiKey}`
);

const soy = new Soy(provider);
```

### Scripting

Scripting with Soy usually looks something like this. This is an example
of creating a new Soy instance and using it to register a domain and publish
the `contenthash` for the site.

```js
const Soy = require('soy-core');
const Web3 = require('web3');
const HDWalletProvider = require('truffle-hdwallet-provider');

// Change these paremeters or pass them in as env variables
const mnemonic = process.env.WALLET_MNEMONIC;
const infuraApiKey = process.env.INFURA_API_KEY;
const infuraNetwork = process.env.INFURA_NETWORK || 'rinkeby';
const contentHash = '/ipfs/QmVyYoFQ8KDLMUWhzxTn24js9g5BiC6QX3ZswfQ56T7A5T';
const domain = 'soyexample.test';

var provider = new HDWalletProvider(
  mnemonic,
  `https://${infuraNetwork}.infura.io/v3/${infuraApiKey}`
);

(async () => {
  const web3 = new Web3(provider);
  const accounts = await web3.eth.getAccounts();
  const owner = accounts[0];

  const soy = new Soy(provider, { from: owner });

  const resolver = await soy.registerDomain(domain);
  const revision = await resolver.publishRevision(contentHash);

  console.log(`Revision ${revision} published by Soy!`);
})().catch(console.log);
```

### View Your Beautiful Site

Once you have ENS set up to point to an ipfs hash, simply add `.soy` to the ENS
domain in your browser. For example, web3studio.eth becomes
[web3studio.eth.soy][web3studio.eth.soy].

## Packages

Soy consists of a bunch of tools that make hosting distributed web sites easy. They are:

### [`soy-contracts`][soy-contracts]

Contracts contains the source of the solidity contracts and a low level interface
for interactions via [`truffle-contract`][truffle-contract].

For more information, see [`soy-contracts`][soy-contracts]'s main page.

### [`soy-gateway`][soy-gateway]

The gateway is the source code behind eth.soy. It's a shim to enable browsers to
support distributed file systems over ENS until browsers can handle this natively.

For more information, see [`soy-gateway`][soy-gateway]'s main page.

### [`soy-core`][soy-core]

The core project contains a friendly js interface to interacting with the deployed
contracts of [`soy-contracts`][soy-contracts] enabling you to get your content out there with ease.

## API

### Classes

<dl>
<dt><a href="#Ens">Ens</a></dt>
<dd><p>Soy&#39;s ENS resolver which caches all results per domain&#39;s TTL set by it&#39;s resolver.</p>
</dd>
<dt><a href="#Resolver">Resolver</a></dt>
<dd><p>A nod specific resolver</p>
</dd>
<dt><a href="#Soy">Soy</a></dt>
<dd><p>Soy is the best interface for Soy&#39;s smart contracts. It provides an easily
scriptable interface for any deployment pattern.</p>
</dd>
</dl>

<a name="Ens"></a>

### Ens

Soy's ENS resolver which caches all results per domain's TTL set by it's resolver.

**Kind**: global class

- [Ens](#Ens)
  - [new Ens(provider, [registryAddress])](#new_Ens_new)
  - [.resolver(domain)](#Ens+resolver) ⇒ <code>Promise.&lt;SoyPublicResolver&gt;</code>
  - [.getContentHash(domain)](#Ens+getContentHash) ⇒ <code>Promise.&lt;string&gt;</code>

<a name="new_Ens_new"></a>

#### new Ens(provider, [registryAddress])

Constructor

| Param             | Type                | Description                                       |
| ----------------- | ------------------- | ------------------------------------------------- |
| provider          | <code>Object</code> | A web3@1 provider, defaults to localhost          |
| [registryAddress] | <code>string</code> | An optional registry address for bespoke networks |

**Example** _(Get the &#x60;contenthash&#x60; for a domain)_

```js
const siteHash = soy.ens.getContentHash('web3studio.eth');
```

<a name="Ens+resolver"></a>

#### ens.resolver(domain) ⇒ <code>Promise.&lt;SoyPublicResolver&gt;</code>

Gets a resolver contract instance for a registered ENS domain

**Kind**: instance method of [<code>Ens</code>](#Ens)  
**Returns**: <code>Promise.&lt;SoyPublicResolver&gt;</code> - Resolver for a domain

| Param  | Type                | Description                     |
| ------ | ------------------- | ------------------------------- |
| domain | <code>string</code> | ENS domain (eg: web3studio.eth) |

<a name="Ens+getContentHash"></a>

#### ens.getContentHash(domain) ⇒ <code>Promise.&lt;string&gt;</code>

Resolves the `contenthash` for an ENS domain

**Kind**: instance method of [<code>Ens</code>](#Ens)  
**Returns**: <code>Promise.&lt;string&gt;</code> - The `contenthash` for the ENS domain

| Param  | Type                | Description                     |
| ------ | ------------------- | ------------------------------- |
| domain | <code>string</code> | ENS domain (eg: web3studio.eth) |

<a name="Resolver"></a>

### Resolver

A nod specific resolver

**Kind**: global class

- [Resolver](#Resolver)
  - [new Resolver(domain, resolver)](#new_Resolver_new)
  - [.publishRevision(contentHash, [alias], [txOps])](#Resolver+publishRevision) ⇒ <code>Promise.&lt;number&gt;</code>
  - [.contenthash()](#Resolver+contenthash) ⇒ <code>Promise.&lt;string&gt;</code>

<a name="new_Resolver_new"></a>

#### new Resolver(domain, resolver)

Create a unique contract instance with common params filled in.
Wraps all methods of [SoyPublicResolver](https://github.com/ConsenSys/web3studio-soy/blob/master/packages/soy-contracts/contracts/SoyPublicResolver.sol)
and by extension the base [PublicResolver](https://github.com/ensdomains/resolvers/blob/master/contracts/PublicResolver.sol)
without the need to specify a namehashed domain and tedious unit conversions.

[`truffle-contract`](https://github.com/trufflesuite/truffle/tree/next/packages/truffle-contract)
is used to generate the interface. For more detailed explanations, see their
[docs](https://truffleframework.com/docs/truffle/getting-started/interacting-with-your-contracts)

| Param    | Type                           | Description         |
| -------- | ------------------------------ | ------------------- |
| domain   | <code>string</code>            | ens domain          |
| resolver | <code>SoyPublicResolver</code> | A resolver contract |

<a name="Resolver+publishRevision"></a>

#### resolver.publishRevision(contentHash, [alias], [txOps]) ⇒ <code>Promise.&lt;number&gt;</code>

Publishes the content hash as a revision

**Kind**: instance method of [<code>Resolver</code>](#Resolver)  
**Returns**: <code>Promise.&lt;number&gt;</code> - The revision number

| Param       | Type                | Description                           |
| ----------- | ------------------- | ------------------------------------- |
| contentHash | <code>string</code> | Content hash to publish for your site |
| [alias]     | <code>string</code> | alias to set for this hash            |
| [txOps]     | <code>Object</code> | web3 transactions options object      |

<a name="Resolver+contenthash"></a>

#### resolver.contenthash() ⇒ <code>Promise.&lt;string&gt;</code>

Get the current contenthash

**Kind**: instance method of [<code>Resolver</code>](#Resolver)  
**Returns**: <code>Promise.&lt;string&gt;</code> - current resolver content hash  
<a name="Soy"></a>

### Soy

Soy is the best interface for Soy's smart contracts. It provides an easily
scriptable interface for any deployment pattern.

**Kind**: global class  
**Properties**

| Name | Type              | Description                                                                  |
| ---- | ----------------- | ---------------------------------------------------------------------------- |
| ens  | <code>ENS</code>  | [ENS](#ens) resolver utility                                                 |
| web3 | <code>Web3</code> | [web3.js](https://web3js.readthedocs.io/en/1.0/) instance                    |
| ipfs | <code>IPFS</code> | [ipfs-http-client](https://github.com/ipfs/js-ipfs-http-client#api) instance |

- [Soy](#Soy)
  - [new Soy(provider, [options])](#new_Soy_new)
  - [.uploadToIPFSAndPublish(path, domain, [options])](#Soy+uploadToIPFSAndPublish) ⇒ <code>Promise.&lt;{hash: string, rev: number}&gt;</code>
  - [.resolver(domain)](#Soy+resolver) ⇒ [<code>Promise.&lt;Resolver&gt;</code>](#Resolver)
  - [.registerDomain(domain)](#Soy+registerDomain) ⇒ [<code>Promise.&lt;Resolver&gt;</code>](#Resolver)

<a name="new_Soy_new"></a>

#### new Soy(provider, [options])

Create a new soy instance

| Param                     | Type                       | Description                                                                                                        |
| ------------------------- | -------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| provider                  | <code>Web3.Provider</code> | A Web3 provider instance                                                                                           |
| [options]                 | <code>Object</code>        | Soy instance options                                                                                               |
| [options.registryAddress] | <code>string</code>        | An address for a deployed ENS registry                                                                             |
| [options.resolverAddress] | <code>string</code>        | An address for a deploy SoyPublicResolver                                                                          |
| [...options.txOps]        | <code>Object</code>        | Default [transaction arguments](https://web3js.readthedocs.io/en/1.0/web3-eth.html#sendtransaction) passed to web3 |

<a name="Soy+uploadToIPFSAndPublish"></a>

#### soy.uploadToIPFSAndPublish(path, domain, [options]) ⇒ <code>Promise.&lt;{hash: string, rev: number}&gt;</code>

Upload the contents of a directory to ipfs and publishes the root folder's
hash as a revision

**Kind**: instance method of [<code>Soy</code>](#Soy)  
**Returns**: <code>Promise.&lt;{hash: string, rev: number}&gt;</code> - - The hash published and it's revision number

| Param     | Type                | Description                                                                                     |
| --------- | ------------------- | ----------------------------------------------------------------------------------------------- |
| path      | <code>string</code> | Path to the directory                                                                           |
| domain    | <code>string</code> | ENS domain to publish a revision                                                                |
| [options] | <code>Object</code> | IPFS [options](https://github.com/ipfs/interface-ipfs-core/blob/master/SPEC/FILES.md#addfromfs) |

<a name="Soy+resolver"></a>

#### soy.resolver(domain) ⇒ [<code>Promise.&lt;Resolver&gt;</code>](#Resolver)

With a registered domain, get a resolver instance for a specific node

**Kind**: instance method of [<code>Soy</code>](#Soy)  
**Returns**: [<code>Promise.&lt;Resolver&gt;</code>](#Resolver) - - A resolver instance

| Param  | Type                | Description             |
| ------ | ------------------- | ----------------------- |
| domain | <code>string</code> | The domain for the node |

**Example** _(Publish a revision of your site)_

```js
const resolver = await soy.resolver('example.madewith.eth');

await resolver.publishRevision(
  '/ipfs/QmVyYoFQ8KDLMUWhzxTn24js9g5BiC6QX3ZswfQ56T7A5T'
);
```

<a name="Soy+registerDomain"></a>

#### soy.registerDomain(domain) ⇒ [<code>Promise.&lt;Resolver&gt;</code>](#Resolver)

Registers a new domain and sets it's resolver to Soy's PublicResolver
contract. This will only need to be done once per (sub)domain

If you haven't done so yet, you will need to purchase a domain. We
recommend using [My Ether Wallet](https://www.myetherwallet.com/#ens).
Domain auctions will last a week.

**Kind**: instance method of [<code>Soy</code>](#Soy)  
**Returns**: [<code>Promise.&lt;Resolver&gt;</code>](#Resolver) - a resolver instance

| Param  | Type                | Description                  |
| ------ | ------------------- | ---------------------------- |
| domain | <code>string</code> | a new ENS domain to register |

**Example** _(Register an ENS Domain with Soy)_

```js
const resolver = await soy.registerDomain('example.madewith.eth');
```

## Contributing

Please read through our [contributing guidelines][contributing].
Included are directions for coding standards, and notes on development.

## License

[Apache 2.0][license]

[soy-contracts]: https://github.com/ConsenSys/web3studio-soy/tree/master/packages/soy-contracts
[soy-gateway]: https://github.com/ConsenSys/web3studio-soy/tree/master/packages/soy-gateway
[soy-core]: https://github.com/ConsenSys/web3studio-soy/tree/master/packages/soy-core
[license]: https://github.com/ConsenSys/web3studio-soy/blob/master/packages/soy-core/LICENSE
[contributing]: https://github.com/ConsenSys/web3studio-soy/blob/master/packages/soy-core/CONTRIBUTING.md
[truffle-contract]: https://github.com/trufflesuite/truffle/tree/next/packages/truffle-contract
[web3studio.eth.soy]: https://web3studio.eth.soy
