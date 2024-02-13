---
title: Detect Web3 Frontend Attacks with dAppling DNS Monitor
description: There's been 5+ DNS attacks in the last few months. We built a tool to help protect users. 
author: Russell
date: 2024-01-25
tags:
  - 'dns'
  - 'security'
  - 'tooling'
  - 'dns'
---
**TL;DR:** We built [dappling.network/monitor](https://dappling.network/monitor) to help prevent web3 DNS hijacking attacks. This tool alerts you to potential DNS hijacking. By monitoring over 3,000 Web3 domains (everything on [DeFiLlama](https://defillama.com)) for nameserver changes, it helps you be aware when domains are hijacked. [Try the tool now](https://dappling.network/monitor) and be part of a safer DeFi community.

![The DNS Monitor Tool](/images/dns-monitor/monitor-demo.png)

# Why is this needed? 
In the last few months there have been several DNS hijacking attacks targeting web3 protocols. These included [Frax](https://x.com/fraxfinance/status/1719497560543658073?s=20), [Balancer]( https://twitter.com/Balancer/status/1704281611326357567), [Galxe]( https://twitter.com/galxe/status/1710305141016944654), [Velodrome]( https://twitter.com/VelodromeFi/status/1730040745736683679), and [Aerodrome](https://x.com/aerodromefi/status/1736780326070870072?s=20) to name a few. 

They all happened the exact same way. The DNS registrar they were using was social engineered, and the hacker was able to change the nameservers and take control over the domain.

These nameserver changes is exactly what the tool looks for and would have detected all 5 of those attacks. 

# How it works?
We used [DeFiLlama](https://defillama.com) to build our original lists of sites to monitor. We then use cloudflare's [DNS over HTTPS api](https://developers.cloudflare.com/1.1.1.1/encryption/dns-over-https/) to check each of the nameservers for changes every 5 minutes. If we detect a change, we send out a notification to everyone who subscribed to nameserver changes for that domain + update the monitoring table. 

We also automatically add all dappling.network production domains to the monitor list + they get extra features to help them protect their users against these types of attacks.

# Next Steps
* [Check out the tool](https://dappling.network/monitor) + signup for notifications for your most used site
* Deploy your site on [dAppling](https://dappling.network) to get this + other security features with 0 config. 
* [DM me on twitter](https://twitter.com/0xbookland) if you have questions or want a site that's missing added.  

# Video Demo
[Video](https://x.com/0xBookland/status/1757493370962739438?s=20) 

# Thank you
Thank you to the [dAppling team](https://dappling.network) for building the tool. CerealSabre from [eth.limo](https://twitter.com/eth_limo) for helping me brainstorm this idea and [Derrick](https://twitter.com/pcfreak30) from [Lume](https://twitter.com/LumeWeb3) for helping me understand DNS at a deeper level. Also huge shout out to the [3DNS team](https://3dns.box/) who's helping reduce the DNS hijacking problem with their hybrid domains. 