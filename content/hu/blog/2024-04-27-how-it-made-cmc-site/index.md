---
title: "Fejlesztői hétvége: Webprojekt készítése a semmiből"
description: "A step-by-step guide to building and deploying a website with modern development tools.."
summary: "From local setup to live site: documenting a web developer's process using GitHub and Netlify."
date: 2024-04-27
# lastmod:
draft: false
weight: 50
categories: [OpenSource]
tags: [
  TechTutorial,
  OpenSource,
  Hugo,
  BuildingInPublic,
  MentalInCyber
]
contributors: [Zoltan Toma]
pinned: false
homepage: false
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

It's Saturday afternoon, and I've sat down in front of my computer. I'm considering starting on the website for my "hobby" project. While doing this, I wonder what others do in their free time... But let's get started.

Yesterday, I launched the new site for GoSecNinja and updated a few DNS settings. The result:

![](cover_20240427152246.png)

> Source: [https://gosecninja.com](https://gosecninja.com) (Yes, this is where the ad goes :D)

Today, I want to achieve the same with Curious Minds Collective. Let's start at the beginning.

## Repository

The source for the site will be on GitHub.

![](20240427152748.png)

For now, this will be a public repository, possibly under the MIT license. I still need to think about that, but I want what I write here to help as many people as possible.

After that, we can clone it and start working on it. I won't go into git clone, commit, push, etc... Just so you know, I use GitKraken to pull the repos and manage changes.

![](20240427153910.png)

A note here: I've cloned this repo into the main Obsidian Note library. Why? Because it allows me to just move my thoughts from one folder to another if I decide to publish it.

> A tip for those who want to host their site on GitHub. If you add "github.io" after the desired repo name, you can view your static content at `https://{repo_name}.github.io`. I named the repo differently because I'm publishing it differently.

## The chosen theme

Since I last spent a lot of time pondering how my website should look but I'm not a front-end developer, I want to start with something simple. Therefore, I'm using Hugo for generating the site, but how it will look is another question entirely.

For many other reasons, but now the Doks theme has caught my attention. I'm not attaching a picture here because I'm not going to change it just yet. However, what can be achieved with it is truly amazing:

![](20240427155336.png)

> Source: https://getdoks.org/showcase/

However, this is not a Hugo theme, but a web framework and can rather be set up using npm. I don't know about you, but if I can avoid it, I do not install anything on my host machine. That is, the one I'm currently using.

## Development Environment

Therefore, I prefer to run everything from Docker. Different dependencies are in their tiny containers. :D Since I don't want to fuss too much with the settings, I will be using a devcontainer for it in VSCode. (But since I have a Mac, I've had problems with file synchronization, so this won't be the final solution.)

![](20240427160155.png)

You can check out the contents on GitHub... But now I have the opportunity to open everything in a container.

![](20240427160333.png)

Here, npm is available, and I can issue the necessary commands.

```bash
npm create hyas@latest -- --template doks
```

## Project Bootstrap

![](20240427160644.png)

I'll make one change to the results. I don't need this subfolder, so I'll copy everything to the root.

![](20240427160821.png)

```bash
npm install && npm run dev
```

After a little while, the site is running locally.

![](20240427161335.png)

And it's also accessible from the browser:

![](20240427161415.png)

I could commit now... but we don't have a gitignore yet. We can use gitignore.io to generate it. (Good to know that they now redirect to Toptal...-.-)

![](20240427161848.png)

Now it's time to commit and push.

Maybe we could supplement the devcontainer settings with recommended VSCode extensions. (I've included this in the settings.)

## Auto Build

It has been a while since I initially set up my previous sites, so I was wondering how to start. Should I use a GitHub pipeline or a local build? But Netlify will do the job.
We just need to add a new site.

![](20240427163511.png)

The simplest way is to deploy with GitHub. You have plenty of free build time. (But you can also create a build "server" for it. But that also another topic.)

![](20240427163543.png)

Grant Netlify access to the Github repo. (I prefer to only grant access to specific resources.)

![](20240427163730.png)

Setup deploy settings

![](20240427163941.png)

Wait till the build completes.

![](20240427164357.png)

I was thinking about at this point, I should obfuscate the Netlify backend image or not. (I could change it any time if I want...) Now let's see if it's available.

![](20240427164502.png)

Yes, it is out. ˆˆ

## Domain Settings

Now it's time for domain settings. First, you need a domain name... but I already have one.

I use Porkbun... why?

![](20240427164903.png)

Because I can set it to only allow login with a hardware key...

![](20240427164701.png)

Add domain.

![](20240427165251.png)

Set up Cloudflare

![](20240427165358.png)

Add domain.

![](20240427165505.png)

Free is good enough.

![](20240427165540.png)

Change DNS servers in Porkbun.

![](20240427165718.png)

HTTPS Security? Use Cloudflare!

![](20240427165923.png)

![](20240427165942.png)

![](20240427170003.png)

![](20240427170017.png)

And the result is... tatarataaaa:

![](20240427172731.png)

B plusz. It's not bad. When I think about sites that still don't have an HTTPS connection, I wonder why.

Although the result is worse than a plain Netlify page:

![](20240427172648.png)

Because Cloudflare strips some of the headers when content is distributed in the CDN.(I'll be working on this later...)

You can check your scores at: https://observatory.mozilla.org

But let's continue... Remove imported DNS records.

![](20240427170245.png)

![](20240427170325.png)

Add DNS settings provided by Netlify.

![](20240427171114.png)

And the site is now available at the domain:

![](20240427171213.png)

## Customizing the Site

Now that the site is up, we can start making some improvements. Maybe delete unnecessary content. These are all in the git history anyway.

![](20240427184816.png)

And we could create a blog post. You know... a kind of hello world.

But you know what? It could be something about how I made this site. What I'm actually writing right now. It will be the first content on the site. :D

## First blog post

All I need is to add a frontmatter.

![](20240427185102.png)

Then, move the folder to the new site repository.

![](20240427185228.png)

(Just in case, I renamed the folder to `2024-04-27-how-it-made-cmc-site`)

Now I just need to rename the screen captures because `Pasted image 20240427185228.png` is not so good.

To do this, I use this command in a VSCode terminal:

```bash
ls *.png | while read a; do n=$(echo $a | sed -e 's/^Pasted image //'); mv "$a" $n; done
```

Now I need to rename the Obsidian embeddings in the note... or now I can call it in the post.

For this, I will use a regex search in the content to find and correct every pasted image.

```regex
Match: !\[\[Pasted image (.*)\]\]
Replace: ![]($1)
```

And now it is time to ask ChatGPT about how to call this post and what tags to use. I will probably use something different..., but it has some great ideas... sometimes.

## Call To Action?

So, curious minds, how do you spend your free time?

I’ve been thinking a lot about an impactful call to action section. The results? Well, basically, I’ve been constantly looking at various statistics, emails, and social network sites.

So, if you liked what I wrote, please:

- DO NOT clap me!
- DO NOT hit a like!
- DO NOT share it!
- DO NOT want to talk to me on Calendly: https://calendly.com/zoltantoma87/one-on-one, I can only handle one person per day for up to an hour.
- DO NOT add me as a friend on LinkedIn: https://www.linkedin.com/in/toma-zoltan/
- DO NOT support me on Patreon: https://www.patreon.com/CuriousMindsCollective
- DO NOT want to work with me! I have unrealistic ideas about working together: https://medium.com/@zoltantoma/how-much-should-i-charge-for-my-service-b2c8b380e66e

And DO NOT! I repeat, DO NOT follow my work. It would only feed my own Ego while I am working on the #MentalInCyber topic and trying to convey the (cyber) security-first business model.

## PS

I almost forgot... the code is available on github. [https://github.com/gosecninja/curious-minds-collective](https://github.com/gosecninja/curious-minds-collective)

(And I need to look into why the links aren't showing up properly in the post... a todo for next time)
