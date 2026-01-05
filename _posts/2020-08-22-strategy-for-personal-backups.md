---
layout: post
title: Strategy for personal backups
categories: 
    - code
last_modified_at: 2020-08-22T14:28:00+01:00
excerpt_separator: "<!--more-->"
---

Here I'm sharing with you my current backup strategy for personal files, which involves using a physical HDD and [AWS S3 service](https://aws.amazon.com/s3/). <!--more-->I currently have about **1 TB** of data that is kept safe. I estimate spending an average of **60 USD per year** with it. But as times passes, technology advances, costs are expected to decrease. I tried over the years different strategies, but with this one I feel the most comfortable so far. I tried to optimize my solution taking into account these 4 main aspects:


- Cost: I don't like spending more than I need. 
- Reliability: All my data will be intact for, at least, as long as I live.
- Safety: Only people I allow will have access to them.
- Convenience: I'd like to have access to my files whenever I need them, share them occasionally with someone else.

## TL;DR

Everything boils down to: 

- A _cold_ backup in the cloud, using AWS S3 Glacier. 
- A _warmer_ backup in a physical 2 TB HDD that is replaced every 5 years, for local redundancy and eventual interaction cost for less frequently accessed files. 

## Why is this a good idea?

In order to answer this question let's go through some of the storage alternatives considered for backup and their pros and cons. 

### HDD and/or SSD

If you only keep your files in hard disks or solid state drives as a long-term backup you might run into trouble, since they deteriorate somewhat quickly. After a period of 5 to 10 years, depending on the device's quality and how careful you handle it, some of the data can start to get corrupted or, in the worst case, the disk might stop working at all. This happens mostly due to the mechanical and electromagnetic components used to build such devices. 

HDDs and SSDs work in very different ways from each other. Each with its cons and pros. When thinking about backup, we need to keep in mind that HDDs are significantly slower than SSDs, but they can retain data slightly longer and are significantly cheaper. Thus, if you don't expect touching a lot of your data very often - which is my case - SDDs are not much more advantageous when compared to HDDs. If you use an HDD as a temporary backup alternative - for instance, for storing files you plan to access more frequently, while having a copy of your files elsewhere, it might not be a problem. Other than hardware failure, another reason for redundancy is that HDDs are physical devices that can be lost or broken. 

The hardware failure problem can be mitigated by making a new backup into a new HDD about every 5 years. This also gives you an opportunity to buy a bigger disk when necessary. The fact I have a cold backup of everything in the cloud makes me comfortable choosing an inexpensive commodity HDD. 

In terms of costs, last time I spent around 70 USD in a simple 2 TB HDD with USB 3.0 connection. Amortized over 5 years, 14 USD is the HDD's fraction of the 60 USD yearly spent on backup.

### Flash sticks and memory cards

Nope. All the same problems with HDD and SSD, but worse. They are essentially SSDs, but cheaper and smaller. They become unreliable much sooner and it's easier to lose or break them. They are optimized for efficient data transfer, never backup.

### CDs or DVDs

In terms of long term integrity, this type of media can be quite good, since good quality CDs and DVDs when properly handled can retain data intact for decades. But not ideal in terms of convenience. Some of the issues with them:

- Fragile against physical impact and environmental conditions.
- Obsolete: DVD drives are becoming harder to find these days. Many of newer laptops don't even come with DVD drives anymore.
- Hassle for organising. You might need big a number of disks if you have hundreds of GB to secure.
- Can be really slow for moving data in and out.

### Cloud 

I think the cloud is the ideal option for long-term data storage for the following reasons:

- Long-term data integrity is _almost_ guaranteed: it's in the best interest of cloud storage providers to do the best they can to keep the data stored in them reliable. They do the hard work of preventing hardware failures for you, like redundancy and using the proper hardware.
- Plenty of options for different needs and cost allocations. Everything depends on how you use a service and on the provider of choice. But, as I show further, with a bit of technical literacy (read as _curiosity_) you can make adjustments to be make it reasonably cheap.
- No need to manage physical objects.
- It's on the internet: it's easier sharing your data with friends, and you can have access to it from anywhere.

Although, as with everything else in life the cloud also comes with drawbacks: 

Nice as it is having the possibility to easily share your data with people you want, it becomes easier to share you data with people you do _not_ want to. In addition, cloud storage providers are companies, they might change policies or pricing, they might to try to sell your data to third-parties, or simply go out of service. It's better to plan ahead for those possibilities. This is one of the reasons why I opt having a redundant copy of my files in an HDD with me.

#### Commercial storage services

If you have enough money, don't care spending it, or really need advanced sharing or collaboration features (which prompts the question that backup is what you really need), there is a bunch of fully-featured commercial cloud storage out there that will do almost everything for you when it comes these usage cases. Here are some of the fully-featured storage services in the wild, and how much money you have - as of today - to pay, approximately, for storing 1 TB of data for a **year** in the cheapest plan possible:

- [Dropbox](https://www.dropbox.com/individual/plans-comparison): 120 USD
- [Box](https://www.box.com/pricing): 170 USD
- [Google One](https://one.google.com/about#upgrade): 150 USD
- [OneDrive](https://www.microsoft.com/en-us/microsoft-365/onedrive/compare-onedrive-plans): 69.99 USD until 1 TB. 99.99 for any amount more than 1 TB.
- [Spider Oak One Backup](https://spideroak.com/one/): 149 USD

It's worth noting that the prices above - exception of OneDrive - allows for a storage size of more than 1 TB. 3 TB in the case of Dropbox, unlimited in the case of Box. For OneDrive, if you need to store more than 1 TB you will have to upgrade to the next plan, which costs 99.99 USD yearly.

From those services I had used, and still use in some form, Dropbox, Google One and OneDrive. They generally seem like good services, but I don't feel comfortable using them as the my last frontier of backup because [this](https://twitter.com/jonoberheide/status/1224525738268905477). Although this can happen with anything connected to the internet, the degree of control (and difficulty to use) provided by a cloud platform (when compared to commercial services) is way higher.

#### Cloud platforms

Another alternative for storing data in the cloud is using a cloud platform, such as AWS, GCP or Azure. The flexibility provided - if enjoyed carefully - allows you to spend much less money in storage with a lot of power and velocity from full-fledge storage services, but they will demand a bit more in preparation and technical prowess.

My cloud of choice is AWS. Mostly because of my previous familiarity with it, but I believe you might be able to reach similar results with other platforms, too. Typically, cloud platforms offer a plethora of services: from computing power, network and storage to everything you can imagine needed for the modern web. Thus, with a bit of knowledge, you can build your own personal storage service. 

A cool thing about them is that their pricing policies are very granular. For example, if you store 50 GB of data you will be charged exactly for that. Increased to 51 GB? No problem. Your extra cost will be only on that extra GB. But you also have to be careful. Things can go out of hand if you don't tune properly your usage needs.

Cost-wise, [AWS S3 Glacier (deep archive)](https://aws.amazon.com/s3/pricing/), for instance, will charge you only 0.00099 USD per GB per month (if stored in one of its cheapest regions). In other words, for 1 TB a year: **11.88 USD**. This is the cheapest storage format provided by AWS This is possible because the Deep Archive mode archives your data in some form of cold storage that it can only be accessible about 12h after the request. 

The costs can increase dramatically if you need to take out your data from it. Here there are 2 costs involved: 

- Requesting data "defrost" from the Glacier to an S3 bucket. Which, in the cheapest method, costs 0.0025 USD per GB.
- Downloading from S3 to your local computer, if you need to take out more than 1 GB a month, costs 0.09 USD per GB above the first GB.

 It means that if I need to download all my 1 TB at once, I will have to spend, roughly, 1000 * (0.0025 + 0.09) = 92.50 USD. On the other hand, as I don't expect having to access ALL my data very often. Let's say every 5 years (in case I lost my HDD) I need to download everything, we can dilute this 92.50 USD to about **18.5 USD** per year.

## Wrapping up

You might have noticed that the estimated costs I provided sum up to 14.00 (HDD) + 11.88 (S3 Glacier storage) + 18.50 (S3 data transfer) = 44.38 USD, not the 60 USD stated in the first paragraph. This happens because there are some side-effect costs in S3 than just storage to deep archive and data retrieval:

- I don't actually "freeze" all the data to Glacier. I leave some of it out in order to be able to share it via internet without having to wait the Deep Archive 12h defrost period. 
- You can't immediately send freshly uploaded data to Glacier. Every file uploaded needs to rest for at least 30 days in S3 before being sent to Glacier. During this period it costs a bit more.
- Moving data around.

It might have become obvious that the flexibility provided by a cloud platform comes with a cost in complexity. Here exemplified by how much more complicated the cost calculation is when compared to a commercial platform. And it doesn't even end there. This complexity continues with understanding how S3 works, tinkering with it, taking proper measures to guarantee information security, etc. But the exercise also taught me a lot on how cloud platforms work in general, other possible use cases, and the limits for personal use. In a future post I will describe in details how I use AWS S3 for storing, retrieving, interacting with my files in a safe manner.
