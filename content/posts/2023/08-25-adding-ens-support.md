---
title: Adding ENS Support to dAppling
description: Something feels right about this solution
date: 2023-08-15
tags:
  - frontend
  - learning
	- ens
author: Bookland
---

While working on [dAppling](https://dappling.network) (helping web3 developers by making decentralized websites easy), we wanted to allow our users to be able to access their site through their ENS name. The ENS community is excited about using and sharing their cool ".eth" names. Here's how we integrated ENS name support into our platform.

## Resolving ENS Names
I bought the ENS name bookland.eth. I set a record that will "point" my name to a website. On the [Brave](https://brave.com/) browser, navigating to [bookland.eth](https://bookland.eth) will automatically look up that record and load the content. This is done by going through the "dweb.link" gateway. 

If you're not on brave, a popular option is [eth.limo](https://eth.limo). You can use this service simply by prepending your ENS name: [bookland.eth.limo](https://bookland.eth.limo). Your request is served by first looking at the subdomain (for this example, bookland) and doing a record lookup on ENS. That record is then decoded into the "pointer" I set. Finally, the content is retrieved from IPFS and returned.

## How this Technically Works

Ok - so let’s go though how this feature works. We introduced three different services with this feature: a name service, an ENS records service, and key storage service.

### Name Service
These "pointers" I'm referring to are actually InterPlanetary Name System ([IPNS](https://docs.ipfs.tech/concepts/ipns/)) records. These are cool because we can actually change the mapping of these keys to the content. This is perfect for our use-case of updating a website on behalf of a user. 

We considered 3 different name services
#### dwebservices.xyz
https://dwebservices.xyz/ would have been the simplest option. They provide and API where you can create an IPNS name, and use the API to control where it points and update revisions. This is the simplest option by far if you are OK not having custody of your keys.
Unfortunately, for our use case, we needed to have custody over the IPNS keys. This is because we need to ensure that we have the ability to update where the IPNS names are pointing indefinitely into the future, and we need to be able to ensure the IPNS records are always resolvable.

#### Running the IPNS infra ourselves

The second thing we looked into was what it would look like to run the IPNS infrastructure ourselves. Unfortunately the documentation to do this in a way with high uptime was REALLY bad. I learned that js-ipfs was recently depreciated and is being replaced by Helia. But lacked good documentation around how to do [IPNS on Helia](https://github.com/ipfs/helia-ipns). There are still several questions I have around how to do this in a good way, but decided to move on when I discovered w3name

#### w3name
W3name helps us by making the creation, updating, and deletion easier. Additionally, w3name enables the lookup of the values of these pointers, ensuring that they are always available. We would have control over the keys, and the service has a simple and documented [API](https://web3-storage.github.io/w3name/). Feel free to [read more about w3name](https://web3.storage/products/w3name/) on their site.

### ENS Service
For the interactions with ENS names, we do contract interactions with their [PublicResolver](https://docs.ens.domains/contract-api-reference/publicresolver) using wagmi. Actually, the contract interactions were better than I expected. using the [wagmi CLI](https://wagmi.sh/cli/getting-started), React hooks for each call are created, keeping our code nice and type-safe. We also used some utils from [@ensdomains/content-hash](https://www.npmjs.com/package/@ensdomains/content-hash) and the awesome, also type-safe, [viem](https://viem.sh). 

One complaint with the ENS development is there is no nice way to resolve the content-hash. There are great ways to [resolve names](https://docs.ens.domains/dapp-developer-guide/resolving-names) and such, though.

### Key Storage Service
For key storage, our thoughts were pretty straightforward. How do we keep our users' keys safe while being accessible. Our infrastructure is on AWS so we considered using our Dynamo database, but that did not seem secure. What we ended up with was using storing them encrypted through [AWS ParameterStore](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html). When the keys need looked up, we can retrieve them through our services while keeping them secure for the user.

## Put it Together, What do you Get?
From the user's perspective, it should be simple. 
1. Show intent of adding their ENS domain. In the background we: get the latest IPFS CID, create an IPNS record, send it to w3name, update the project.
2. The user connects their wallet and signs a transaction to update the content hash. Again, in the background we are: verifying the user owns the ENS domain or anything else that would prevent the transaction from succeeding, and creating that transaction with the newly created IPNS key.

Then, immediately after the content record is updated, the user can visit their newly hosted website via Brave, eth.limo, or hopefully soon, anywhere! Even better, every time the project has an update, the content is automatically updated. Let's go!

Go forth and update your ENS records on your dAppling projects!

📚
