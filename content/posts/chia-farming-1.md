---
title: "Chia Farming Setup"
date: 2021-05-08T16:21:38-04:00

tags: [cryptocurrency, chia, farming, mining, homelab]
categories: [chia, cryptocurrency, homelab]
---

For a while I have wanting to set up a homelab of some sort to build my knowledge up in a fun little environment that 
doesn't have the same rules as there are at work. A month ago I read about Chia and the differences in ~~mining~~ 
[farming](https://www.chia.net/greenpaper/) it compared to major cryptocurrencies like Bitcoin and Ethereum. 
Coincidentally I also just finished a new PC build (sans GPU, due to the aforementioned BTC and ETH), so I had the spare
parts to get started. With a leftover i5-6600k and parts, all I needed was the storage, so I bought:

* 1x 2TB Sabrent Extreme Performance NVMe m.2 SSD ($300)
* 2x 12TB Western Digital Elements External HDD ($230/ea)

Let me be the first to say, cleaning the dust and crusted thermal paste out of a 5 year old build is more fun than it 
sounds. The green-ness factor of Chia is the most exciting. Anything that is potentially more profitable than Bitcoin
mining without [using more power than Argentina, Sweden, or Malaysia](https://www.bbc.com/news/technology-56012952) 
is cool in my book. 

The filesystem was configured in a way to keep pre and post-pooling organization as simple as possible, but with 
~7TiB used so far and a pooling release date of May 17th, I'll likely have to come up with another plan. Oh well.

![# pvscan](/images/chia/chia-pvscan-1.png)

My investment rationale was simple. If I failed to get lucky early or home miners got priced out before 
paying off my initial investment, then I can easily use the parts for another project. And if I win, I'll double down 
with a better CPU, a second plotting SSD, and more HDD storage. It literally can't go tits up.

Progress Report:
![Current Chia Status. Nice.](/images/chia/chia-farmer-1.png)

Next up:
* Building a Plex server (if Chia fails me)
* Managing a Chia miner via CLI (if it doesn't)

Read more about Chia [here](https://www.chia.net/).