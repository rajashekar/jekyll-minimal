---
title: "Steps to create decentralized website"
date: 2021-10-12T00:00:00+00:00
author: Rajashekar Chintalapati
layout: post
toc: true
permalink: /decentralized-web/
categories: decentralized
tags: [blockchain, decentralized]
---
In this blog, we will discuss on how to create decentralized website/blog like [this](https://rajashekar.org) one using below steps. <br>

# Generating Content ID (CID)
First you need to create ipfs CID (content id) by using IPFS [InterPlanetary File System](https://ipfs.io/) is a peer-to-peer network protocol for storing and sharing data in a distributed file system, with addresses based on content, not location. <br><br>
There are 2 ways to do it, either <br>

1. [Through running your own ipfs node](#through-running-your-own-ipfs-node) 
2. [Uploading folder to Pinata cloud](#uploading-folder-to-pinata-cloud) (easy way)
## Through running your own ipfs node
1. Install ipfs <br>
    - For mac - `brew install ipfs` <br>
    - For Ubuntu Linux - `snap install ipfs` <br>
2. Initialize ipfs - `ipfs init` <br>
3. Run ipfs daemon - `ipfs daemon` <br>
4. Go to http://localhost:5001  <br>
5. At this point you can upload your file/folder that contains index.html of your site/blog <br><br>
> Note: You can use website generators like [Nikola](https://getnikola.com/) or [Jekyll](https://jekyllrb.com/) or [Hugo](https://gohugo.io/). For Complete list of site generators you can check [jamstack.org](https://jamstack.org/generators/)<br>
This blog was built using Nikola <br>
6. To upload go to http://localhost:5001 and click on files and click on import and choose file or folder that has index.html <br>
    <br><img src="{{ site.baseurl }}/assets/images/ipfs-folder-upload.png" width="70%" style="display: block;margin-left: auto;margin-right: auto;"> <br>
7. Once uploaded and pined, ipfs CID will be created. <br>
    <br><img src="{{ site.baseurl }}/assets/images/ipfs-copy-cid.png" width="70%" style="display: block;margin-left: auto;margin-right: auto;"> <br><br>
> Note: You need to keep your node running for CID to be accessible. <br>
You can use VPS (Virtual Private Server) from [Digital Ocean](https://www.digitalocean.com/) or [Linode](https://www.linode.com/) or [Vultr](https://www.vultr.com/) or [Amazon Light Sail](https://aws.amazon.com/lightsail/) or home server <br> 
to keep ipfs node running
---

## Uploading folder to Pinata cloud
With this approach you dont need to install ipfs locally <br>
Create account in [Pinata Cloud](https://pinata.cloud/) and upload your file or folder that has index.html to pinata cloud and copy the CID, like below <br>
<br><img src="{{ site.baseurl }}/assets/images/pinata-upload.png" width="50%" style="display: block;margin-left: auto;margin-right: auto;"><br>

---

# Accessing Content through CID
Now its time to access/share your content through CID <br>

## Accessing through gateways 
You can share CID to anyone and they can access through below gateways <br>

1. Through ipfs gateway - use `https://ipfs.io/ipfs/<YOUR-CID>/` <br>
2. Through cloudflare ipfs gateway - use `https://cloudflare-ipfs.com/ipfs/<YOUR-CID>/`

## Accessing through custom domain
Above step is good, but you wont be remembering the CID will be difficult so how about accessing through custom domain <br>

1. Buy a domain from [Google domains](https://domains.google/) or [Namecheap](https://www.namecheap.com/) <br>
2. Best way is to maintain domain using Cloudflare (it will give SSL for free for your domain). Create Cloudflare account and add your domain <br>
3. Go to dns tab <br>
    <br><img src="{{ site.baseurl }}/assets/images/cloud-flare-dns-tab.png" width="70%" style="display: block;margin-left: auto;margin-right: auto;"><br>
4. Add these records in DNS. <br>
    - Add `CNAME` record for your domain & www and point that to `cloudflare-ipfs.com` <br>
    - Add `TXT` record with the name `_dnslink.your.website` and value `dnslink=/ipfs/<YOUR_CID>` <br>
    <br><img src="{{ site.baseurl }}/assets/images/cloud-flare-ipfs.png" width="70%" style="display: block;margin-left: auto;margin-right: auto;"><br>

Now you should be able to access your site through custom domain.

## Accessing through ENS domain
If you want to go further to access your content through Ethereum blockchain you can use [Ethereum Name Service (ENS)](https://docs.ens.domains/) <br>

1. Register your name on [ENS Domains](https://app.ens.domains/).
> Note: You need to have Ethereum Wallet to proceed further, you can use [Metamask](https://metamask.io/) Chrome plugin to purchase Ethereum. This will handle all the Ethereum transactions <br>

2. Once you have Metamask installed in Chrome, you can follow the steps to register your name in ENS <br>
    <br><img src="{{ site.baseurl }}/assets/images/ens-steps.png" width="70%" style="display: block;margin-left: auto;margin-right: auto;"> <br>
3. Once above step is done, you can add your ETH/BTC/LTC public address, so that this namespace can be used for crypto transactions. <br>
4. Here you can add your content `ipfs://<YOUR-CID>`. This will create a `https://<NAME>.eth.link`. You can use this link to access your content. <br>
    <br><img src="{{ site.baseurl }}/assets/images/ens-content.png" width="70%" style="display: block;margin-left: auto;margin-right: auto;"><br>
> Note: Publishing this way is expensive since this requires to pay [gas price](https://www.investopedia.com/terms/g/gas-ethereum.asp) every time you modify your site. <br>
5. Anyone with your .eth domain can access your content, currently there are few browsers like [Brave](https://brave.com/) or [Opera](https://www.opera.com/) can be used to access .eth domains. If you visit any .eth domain in Brave, you will be given below message to enable ENS. Might be in future other browsers will adopt ENS. 
    <br><img src="{{ site.baseurl }}/assets/images/brave-eth.png" width="70%" style="display: block;margin-left: auto;margin-right: auto;"><br>

---

## Deploying content in multiple nodes
One of the advantage of using IPFS protocol is to avoid single point of failure, you can deploy the same content in multiple nodes. Now you have the CID, you can pin the same CID in multiple nodes <br>
1. If you have CID from pinata.cloud, you can pin the CID in another node by using command  - `ipfs pin add <YOUR-CID>`
    <br> or through UI <br>
    <br><img src="{{ site.baseurl }}/assets/images/ipfs-cid-ui.png" width="70%" style="display: block;margin-left: auto;margin-right: auto;"> <br>
2. If you have CID through your node, then you can upload to pinata.cloud, like below <br>
    <br><img src="{{ site.baseurl }}/assets/images/pinata-cid.png" width="70%" style="display: block;margin-left: auto;margin-right: auto;"> <br>

---

## Automate publish
You can use [IPFS deploy](https://github.com/ipfs-shipyard/ipfs-deploy) to automate publishing to different IPFS clouds like [Infura](https://infura.io/), [Pinata](https://pinata.cloud/), [Fission](https://fission.codes/) and also set Cloudfare DNS settings.<br>

---
## Conclusion
Done now you have your website/blog running using peer-to-peer network protocal IPFS. <br>
You can access this blog through <br>

- IPFS Cloudflare - [https://rajashekar.org/posts/steps-to-create-decentralized-website](https://rajashekar.org/posts/steps-to-create-decentralized-website) <br>
- Ethereum blockchain - [https://rajashekar.eth.link/posts/steps-to-create-decentralized-website](https://rajashekar.eth.link/posts/steps-to-create-decentralized-website) <br>

Hope this helps.