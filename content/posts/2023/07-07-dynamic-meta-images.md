---
title: A Picture Haiku: Meta Image Generation in Next.js
description: Using @vercel/og and gradients to create web haikus.
date: 2023-07-07
tags:
  - frontend
  - learning
  - art
author: Namaskar 🙏
---
When I stroll through the web without intention, every link, title, and image vies for my attention. When going with a goal in mind, those same links, titles, and images seem to lack dimension. That is to say, I fail to find the information I seek; the information behind each new possible link has no context, and I'm left blind.

Though, there is a glimmer of hope. The meta tags of a website. Used to share information about potential content behind a link, these small tags can hold enormous amounts of useful data. As long as they are used correctly, that is.

## Essence of Representation
Meta tags provide glimpses into the content without requiring users to visit the page itself. These tags can articulate essential details like the title, description, and intended size of the site. With the popularization of social media, Facebook introduced Open Graph tags, a standard used today. Open Graph tags (og for short) define a way to share basic information, location data, media information, and [more](https://ogp.me/). There is a newer and cool standard called [JSON for Linking Data](https://json-ld.org/) that will not be explored.

The tag for an arbitrary image, `og:image`, I think has the most potential. There are so many possibilities with an image. So many pixels! It can be the cause of an interested click. For dAppling, I wanted to approach it with a desire to give the user as much information as possible. Before you visit the site, you are given the bare essence of representation. The website page distilled.

![](https://github.blog/wp-content/uploads/2021/06/framework-open-graph-images_fig-1-GitHub-twitter-card.png?resize=2400%2C1260)

By way of example, GItHub's meta images. Explained more in their [# A framework for building Open Graph images](https://github.blog/2021-06-22-framework-building-open-graph-images/) article, the image succinctly and, in my opinion, beautifully explains what you will see before you see it. From this little image, I have been able to quickly gather information and confidently ignore or navigate into the site.

## The Alchemist's Wand
My plan was simple, and my path started at a friendly merchant's tent. He was giving away copies of this software [@vercel/og](https://vercel.com/blog/introducing-vercel-og-image-generation-fast-dynamic-social-card-images) for free! That covered the image generation part, and he reached into a chest to pull out [Next.js](https://github.com/vercel/next.js). With these two pieces, the mixing was all that was left.

A flick of the wrist and a creation appeared ![](https://www.dappling.network/api/og?id=blog)![](https://www.dappling.network/api/og?id=dog)
![](https://www.dappling.network/api/og?id=bog)
The above images were created on our new shiny image generator: `https://www.dappling.network/api/og`. Each has a different background you've noticed? That's right. There's a gradient generator tied to the project ID on dAppling. This makes projects instantly recognizable. The top one has `https://www.dappling.network/api/og?id=blog`, middle `https://www.dappling.network/api/og?id=dog`, and bottom `https://www.dappling.network/api/og?id=bog`, 

![](/images/dynamic-meta-images/project-card.png)
So here's our project page. The ID for this one is `meow`, and you'll see the information at a glance. The deployment card adds two elements.
![](/images/dynamic-meta-images/deployment-card.png)
If the build was successful or not, as well as the signature existing. This basically means that someone says, "I believe this site is legit!"

You can see the pretty gradient for each of the cards is the same.

### Next.js
Now we come to the unfortunate story. I would love to say these things all worked perfectly, but we ran into some terrible performance hit from the addition. You see, when apps like telegram, twitter, discord, and the like try to get these images, the data must be created on the fly. This causes a few problems like, how do we get the information for the page on load? 

This is solved by getServerSideProps, but also causes the performance hit. When browsing the site normally, one must wait for the metadata to load, even though they are not concerned with the meta image, after all, they're already on the site.

So we had to scale back. And while we have some ideas of bringing them back, for now, the deployments have less information, but still pretty images.

![](https://www.dappling.network/api/og?type=project&id=meow) 
![](https://www.dappling.network/api/og?id=meow&type=deployment)
