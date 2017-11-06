---
layout:     post
title:      "Team Center isn't a team player"
subtitle:   "Lazy implementation can and will cause data loss"
date:       2016-01-03 15:00:00
author:     "z"
header-img: "img/2015-01-03-linux-books.jpg"
---

Today my CATIA crashed as I tried to save my changes into TeamCenter (PDM). Nothing special, it is just a crashy program - i know it well - so i just opened a new instance, loaded my product, and as I went back to mess with my skeleton, I found something interresting. My skeleton (the base of every geometry) had a new icon what i haven't seen yet before:

<img src="/img/2015-01-03-broken-skeleton.png" />

Hmm... Interresting.
I tried to open it in new window but i wasn't able to do that, i got just different error messages. Hmm.. What's going on?
Maybe the data is broken in my local PDM temp folder. So i deleted all the files.
As i opened again everything from TeamCenter the skeleton had again this strange icon and I had no idea what went wrong, so I opened the local PDM temp folder to check the files and this is, what i've seen:

<img src="/img/2015-01-03-dead-skeleton.png" />

Umm... Wasn't was it a bit bigger? Oh, yes, it was 4,5 MB last time.
Oh, it is clear now. My CATIA crashed as it tried to save the skeleton into the PDM and the server saved some uncomplete, corrupt data.

I can imagine the communication between my computer and the server:

- Client : Oh, hi server, i wan't to save some data!
- Server : Cool! What's the Part Number?
- C: XYZ!
- S: Ok, send the data!
- C: 01110100110110101101
- S: Is that all?
- C: .....
- S: Client, is that all?
- C: .....
- S: (Umm, maybe he had a hard workday and went to sleep. So let me save this data before i forget about it...)

And the server rushed to write everything to the disk without getting any response from the client, made no CRC or other integrity test, just wrote it to the disk.

Now i need to wait to the IT department, to get back the data from the (hopefully) daily backup.

Nice!