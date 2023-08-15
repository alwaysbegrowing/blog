---
title: How dAppling Uses Filebase
description: The slick and svelte pinning service.
date: 2023-06-23
tags:
  - ipfs
  - filebase
  - learning
author: Namaskar 🙏
---

At the core of [dAppling](https://dappling.network), we both build code and host the completed app, ensuring a convenient experience for our users and one akin to centralized hosting for their users.

We interact with Filebase in two ways: putting things into the bucket and taking things out of the bucket. 

## Into the Bucket
In a way, we use 'storage' in the sense popularized by Amazon because Filebase supports s3 APIs to store content. When a build is complete, we generate build artifacts and upload them to Filebase via the API. Once this process completes, we receive a 'receipt' containing the content identifier (CID) or hash of the uploaded content. This CID is stored with the project for use later when content needs to be accessed.

This process is outlined in the following diagram in which a Uniswap developer wants to use dAppling.


![Request diagram of a dappling code build - how do you build a decentralized app](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/amptlikiaxawtx9qoaqo.png)




## Out of the Bucket
When it comes to content access, there are two main methods. Users can either directly access the content via the CID using their local IPFS node or a gateway, or they can use an auto-updating URL created by dAppling. This URL could be an auto-generated link such as `garden-daisy.dappling.network`, or a custom domain configured by the user, like `ipfs.uniswap.org`.

### An aside of configuring a custom domain

![custom domain request diagram using dappling](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/c4gpb37i6p843pliyino.png)


In the first case of accessing the CID directly, the content may or may not be downloaded via Filebase. This is because some other IPFS nodes may have cached the content. However, if the content does not exist, the "pinned" files will be ultimately retrieved at Filebase's nodes.

In the second case, we use Filebase's gateway directly to retrieve the content.


![request diagram of accessing Firebase content via dappling](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/kegbos6y42kf7s10yaih.png)

## How could this be improved?
### Replace the Record Handling
Currently, we use a CDN in front of Filebase's gateway, which also functions as a CDN. This process appears to involve one extra step. But how else could we allow users to use a custom domain that maps to an updatable CID?

I think it might look something like:
- Filebase handles DNS records somehow. I know it involves certificate provisioning, which is a bummer. It would be nice to request a domain `ipfs.uniswap.org` and have it return the records for a user to update.
- Filebase allows updating the CID associated with the domain programmatically. So when the Uniswap code is updated, we can update the domain.

### Subdomain gateways
As I detailed in my article about [IPFS and resource paths](https://dev.to/namaskar-dappling/hosting-static-sites-on-ipfs-46f), it's crucial to stay as close to the user's development environment as possible. Therefore, we stay away from anything hosted at a "path". 

From [consensys IPFS security article](https://consensys.net/diligence/blog/2021/06/ipfs-gateway-security/),
> TL;DR: Path-based IPFS gateways have a critical flaw: They effectively disable one of the essential security features of modern browsers: the same-origin policy.

We therefore prefer having the user bring their domain to host, like using `ipfs.uniswap.org`, or our preview domain like `garden-daisy.dappling.network`. Because of this, we have sites that will not work on path gateways such as `gateway.dappling.network/<CID hash>`. We would prefer the gateway support the subdomain version a la `<CID hash>.gateway.dappling.network`. 

It would be ideal if Filebase supported these subdomains as the access is much faster than any other gateway, but meanwhile we will direct our users towards a subdomain version like `<CID hash>.ipfs.cf-ipfs.com` 

