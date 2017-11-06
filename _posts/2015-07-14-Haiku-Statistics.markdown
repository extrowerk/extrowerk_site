---
layout:     post
title:      "Haiku Statistics"
subtitle:   "Visual Haiku"
date:       2015-07-14 12:00:00
author:     "z"
header-img: "img/stat.png"
---

Some months ago I started to learn programming with [Python](https://www.python.org/), but i don't really like the pre-made programming courses, so I thought I'll be more motivated, if I try to solve a problem, what interesting for me.
I slept some, and i came up with the following idea: there are some voices about the death of my favorite project what I'm following in the last 14 Years. It is the Haiku Project.

What is [Haiku](https://www.haiku-os.org/)?

> HAIKU is an open source operating system currently in development. Specifically targeting personal computing, Haiku is a fast, efficient, simple to use, easy to learn, and yet very powerful system for computer users of all levels. Additionally, Haiku offers something over other open source platforms which is quite unique: The project consists of a single team writing everything from the kernel, drivers, userland services, tool kit, and graphics stack to the included desktop applications and preflets. While numerous open source projects are utilized in Haiku, they are integrated seamlessly. This allows Haiku to achieve a level of consistency that provides many conveniences, and is truly enjoyable to use by both end-users and developers alike.

I had an idea: it is not enough to say, the project is dying. If it is so, i must to show somehow. So i tinkered a bit about this task, and made a conclusion: it is a voluntary project, and everybody can see the [GIT statistics](https://github.com/haiku/haiku/graphs/contributors). But only the changes in the source tree can't show us, what's going on, we need to see, what's going on in the community, because it is also matters. So i made a program to collect the data about the activity of our community.

The Haiku Project have some Mailing lists, the two most popular are the [Haiku General Mailing List](https://www.freelists.org/archives/haiku/) and the [Main Development List](https://www.freelists.org/archives/haiku-development/). So my program downloaded all the mail archives, and counted the mails, what the developers and users sent to the lists. It was pretty easy. But the project have an [IRC](https://en.wikipedia.org/wiki/Internet_Relay_Chat) channel too on Freenode, so I wrote a module, what downloaded all the [IRC archive](http://echelog.com/?haiku), and counted all the comments. It was a bit hard, but i came up with a solution, so i got the data. I wanted to show, what's going on, so I made a nice chart, and made a markers for the previously releases. Here is it:

<div>
    <a href="https://plot.ly/~miqlas/67/" target="_blank" title="Haiku Project activity" style="display: block; text-align: center;"><img src="https://plot.ly/~miqlas/67.png" alt="Haiku Project activity" style="max-width: 100%;width: 1364px;"  width="1364" onerror="this.onerror=null;this.src='https://plot.ly/404.png';" /></a>
    <script data-plotly="miqlas:67" src="https://plot.ly/embed.js" async></script>
</div>

As you can see, the green area is the IRC channel activity, the orange and blue bars are the General and Development Mailing list activity respectfully.

But i wasn't able to stop, so i collected the data about the [donations](http://www.haiku-inc.org/donations.html), to see, when and how many donation the project got, and here is it:

<div>
    <a href="https://plot.ly/~miqlas/124/" target="_blank" title="Haiku Donations" style="display: block; text-align: center;"><img src="https://plot.ly/~miqlas/124.png" alt="Haiku Donations" style="max-width: 100%;width: 1364px;"  width="1364" onerror="this.onerror=null;this.src='https://plot.ly/404.png';" /></a>
    <script data-plotly="miqlas:124" src="https://plot.ly/embed.js" async></script>
</div>

As you can see, the orange area shows the EUR, while the blue one the USD donations respectfully. And of course, the bars are the [release dates](https://en.wikipedia.org/wiki/History_of_Haiku_(operating_system)) from R1Alpha1 till R1Alpha4.

I'll try to keep the charts up to date in the future.

Thats all for now folks.