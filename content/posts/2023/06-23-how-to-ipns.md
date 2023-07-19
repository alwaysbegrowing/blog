---
title: How to | Adding an IPFS link to any domain
description: Using IPNS, we can relate IPFS content to a domain name.
date: 2023-06-23
tags:
  - ipfs
  - ipns
  - learning
author: Namaskar 🙏
---

First, **why would you want this?** IPNS is another way to access content on the IPFS network. Since the link to the content is set on the DNS itself, the attack surface is slightly smaller, as the malicious user would need access at the DNS level to be successful. Also, as it's closer to traditional website access, while also showing the user direct IPFS accessing of the content is happening.

There's more information about [what's IPNS?]([https://docs.ipfs.tech/concepts/ipns/) as well as a great guide about how [resolving DNSLink](https://docs.ipfs.tech/concepts/dnslink/#resolve-dnslink-name) works, but I'll go over the steps in a very high-level manner here.

The next steps you can try yourself, and you should be able to see [Uniswap's Interface](https://github.com/Uniswap/interface/releases/tag/v4.250.0) at the end.

1️⃣ Get your CID for the site you want to share with users. It will look something like `bafybeibngzztaxprxg6w423plup5kihnizu4t4fdcsq73wgqcayvjrxoaq`.     

   - You can use [dAppling](https://dappling.network) to build your code into a CID.

2️⃣ Go to your DNS provider and add a record to your domain. 
  - Choose the domain you want the users to type in. Usually, the root domain makes sense.
  - Add a text record with the subdomain of `_dnslink` and the value set as `dnslink=/ipfs/<CID>`

```
TXT _dnslink   dnslink=/ipfs/bafybeibngzztaxprxg6w423plup5kihnizu4t4fdcsq73wgqcayvjrxoaq
```

If you want a subdomain, e.g. ipfs.example.com, the subdomain used would be `_dnslink.ipfs`

```
TXT _dnslink.ipfs dnslink=/ipfs/bafybeibngzztaxprxg6w423plup5kihnizu4t4fdcsq73wgqcayvjrxoaq
```

3️⃣ After set, you can direct users to the site via IPNS and a gateway. These would be accessible with a gateway, like Cloudflare, using their `ipns` path using subdomains (recommended):
  -  https://example-com.ipns.cf-ipfs.com/#/
  -  https://ipfs-example-com.ipns.cf-ipfs.com/#/

or using paths (not recommended):
  -  https://cloudflare-ipfs.com/ipns/example.com
  -  https://cloudflare-ipfs.com/ipns/ipfs.example.com


**What are the downsides?** For one, the record will not be updated if you push changes to your code. This will need to be either automated (if the DNS supports it) or done manually.

We do not currently try updating the IPNS record as we handle the CID resolution automatically when setting up a domain on dAppling, but we think IPNS is an interesting access point and it's more accessible.