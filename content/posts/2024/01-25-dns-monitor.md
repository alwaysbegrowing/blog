---
title: Find out Web3 Frontend Attacks ASAP
description: Introducting a tool that will help prevent + spread awareness for web3 frontend attacks. 
author: Russell
date: 2024-01-25
tags:
  - 'dns'
  - 'security'
  - 'tooling'
---
**TL;DR:** We built [dappling.network/monitor](https://dappling.network/monitor) to help prevent web3 DNS hijacking attacks. This tool alerts you to potential DNS hijacking. By monitoring over 3,000 Web3 domains for name server changes (everything on [DeFiLlama](https://defillama.com)), it helps you be aware when domains are hijacked. [Try the tool now](https://dappling.network/monitor) and be part of a safer DeFi community.

![The DNS Monitor Tool](/images/dns-monitor/monitor-demo.png)

# Why is this needed? 
In the last few months there have been several DNS hijacking attacks targeting web3 protocols. [Frax](x.com/fraxfinance/status/1719497560543658073?s=20), [Balancer]( https://twitter.com/Balancer/status/1704281611326357567), [Galxe]( https://twitter.com/galxe/status/1710305141016944654), [Velodrome]( https://twitter.com/VelodromeFi/status/1730040745736683679), [Aerodrome](https://x.com/aerodromefi/status/1736780326070870072?s=20), to name a few. 

They all happened the exact same way. The DNS registrar they were using was social engineered, and the hacker was able to change the nameservers and take control over the domain.

This is exactly what the tool looks for and would have detected all 5 of those attacks. 

# How it works?
All dAppling.network sites are automatically added to this list and get extra features to help them protect their users against these types of attacks.

We are monitoring around 3000 different web3 domains. We check their nameservers every 5 minutes, and if we detect a change, we send out a notification to everyone who subscribed to nameserver changes for that domain + show on the tool the last time the nameservers changed. 
# Next Steps
* [Check out the tool](https://dappling.network/monitor)
* Deploy your site on [dAppling](https://dappling.network) to get this + other security features
* [DM me on twitter](https://twitter.com/0xbookland) if you have questions or want a site that's missing added.  

# Thank you
Thank you to the [dAppling team](https://dappling.network) for building the tool. CerealSabre from [eth.limo](https://twitter.com/eth_limo) for helping me brainstorm this idea and [Derrick](https://twitter.com/pcfreak30) from [Lume](https://twitter.com/LumeWeb3) for helping me understand DNS at a deeper level + helping debug some weird DNS things. 