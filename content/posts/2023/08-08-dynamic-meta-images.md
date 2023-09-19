---
title: Poetry in Imagery • Meta Image Generation in Next.js
description: Using @vercel/og and gradients to create web haikus.
date: 2023-08-08
tags:
  - frontend
  - learning
  - art
author: Namaskar 🙏
---

When I stroll through the web without intention, every link, title, and image vies for my attention.  With a goal in mind, those same links, titles, and images seem to lack dimension. I struggle to find the information I seek; behind each possible link lacking context, I'm left blind.

Meta tags of a website are a helpful guide. Used to share information about potential content behind a link, these small tags can hold enormous amounts of useful data. As long as they are used correctly, that is. I hope their use is expanded and can help and inspire creators to put more energy into their own site's images.

## Essence of Representation
Meta tags provide glimpses into the content without requiring users to visit the page itself. These tags can articulate essential details like the title, description, and intended size of the site. Open Graph tags (og), introduced by Facebook, define a way to share a summary of the content, location data, media information, and [more](https://ogp.me/). 

The tag for an arbitrary image, `og:image`, I think has the most potential. These images are not mere visual aids; they are the distilled essence of the web page they represent. A visual haiku. For our app [dAppling](https://www.dappling.network), I wanted to follow GitHub's lead with a desire to give the user as much information as possible. 

![](https://github.blog/wp-content/uploads/2021/06/framework-open-graph-images_fig-1-GitHub-twitter-card.png?resize=2400%2C1260)

By way of example, GItHub's meta images. Explained more in their [A framework for building Open Graph images](https://github.blog/2021-06-22-framework-building-open-graph-images/) article, the image succinctly and, in my opinion, beautifully explains what you will see before you see it. I love these images. From this little representation, I have been able to quickly gather information and confidently ignore or navigate into the site.

## The Alchemist's Wand
Creating these visual haiku is a craft, a magical art. With tools like [@vercel/og](https://vercel.com/blog/introducing-vercel-og-image-generation-fast-dynamic-social-card-images) and [Next.js](https://github.com/vercel/next.js), I started my journey.

A flick of the wrist and a small creation appeared ![](https://www.dappling.network/api/og?id=blog)![](https://www.dappling.network/api/og?id=dog)
![](https://www.dappling.network/api/og?id=bog)
Each project, tied to a unique gradient, becomes recognizable at a glance. To look under the leaves, [here's a gist of the generator](https://gist.github.com/Namaskar-1F64F/c851789c4df7d528c6c08ec60a9c90f8). The examples above use different IDs:
- top `https://www.dappling.network/api/og?id=blog`
- middle `https://www.dappling.network/api/og?id=dog`
- bottom `https://www.dappling.network/api/og?id=bog`, 

Another gesture of the wand; a motion with emotion. Two new cards materialize. You can see the pretty gradient for each of them is consistent (The ID for the following two are `meow`).
![](/images/dynamic-meta-images/project-card.png)
Our beautiful project card contains a preview at a glance. The deployment card adds two more elements.
![](/images/dynamic-meta-images/deployment-card.png)
You can see, without navigating to the site, if your deployment passed or failed. You can get the "verified" badge after someone signs the deployment, attesting its legitimacy. 


### The Poetry and the Pitfalls • Next.js
Now we come to the unfortunate story. I would love to say these things all worked perfectly, but the real-time creation of these images posed performance challenges. The metaphysical had to meet the physical world of server responses and loading times. You see, when apps like telegram, slack, discord, and the like try to get these images, the data must be created on the fly. This causes a few problems like, how do we get the information for the page on load? 

This is solved by [`getServerSideProps`](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props), but also causes the performance hit. In the new app router, the [`generateImageMetadata`](https://nextjs.org/docs/app/api-reference/functions/generate-image-metadata) function should work nicely. However, when browsing the site normally, one must wait for the metadata to load, even though they are not concerned with the meta image, after all, they're already on the site.

We scaled back, yet the essence remains. The images still capture the spirit, albeit in a more simplistic form.

![](https://www.dappling.network/api/og?type=project&id=meow) 
![](https://www.dappling.network/api/og?id=meow&type=deployment)
### Learnings and Such
- Meta tags are first come first used. That means, if you have multiple meta tags on the page, it'll probably use the first one encountered. This confused me because instead of being able to define site-wide metadata, I had to use Next.js's [`<Head>`](https://nextjs.org/docs/pages/api-reference/components/head) element on every page. 
```html
<Head>
	<meta />
	<meta />
</Head>
```
- Having an `svg` as a meta image will silently fail and ruin your day. Make them `png`.
- Using metadata website checkers do not accurately reflect exactly what will happen on each individual social media platform. They all have their own way to display the images. 
	- [open graph debugger](https://en.rakko.tools/tools/9/) - see if the images work
	- [meta debugger](https://chrome.google.com/webstore/detail/meta-debugger/jfpdemgdamgplelnlmaecbonkfgfgomp#:~:text=Meta%20Debugger&text=Debug%20the%20head%20section%20of,icons%20and%20many%20more%20things.) - view tags in chrome dev tools useful for duplicates
- Using local metadata debugging does not accurately reflect the first load of your page. There is a chance the meta tags change as hooks return data. Keep this in mind when using extensions like [localhost open graph debugger](https://chrome.google.com/webstore/detail/localhost-open-graph-debu/kckjjmiilgndeaohcljonedmledlnkij).
- `twitter:` tags are used by more than just twitter.

The full list of image related tags used:
```html
<meta property="og:url" content={url} />
<meta itemProp="url" content={url} />
<meta itemProp="image" content={imageUrl} />
<meta name="image" content={imageUrl} />
<meta property="og:image" content={imageUrl} />
<meta property="og:image:alt" content="dAppling" />
<meta property="og:image:type" content="image/png" />
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />
<meta property="twitter:image" content={imageUrl} />
<meta property="twitter:image:src" content={imageUrl} />
<meta property="twitter:card" content="summary_large_image" />
<meta name="twitter:site" content="@dApplingNetwork" />
<meta name="twitter:creator" content="@dApplingNetwork" />
<meta property="twitter:url" content="https://www.dappling.network" />
<meta property="twitter:title" content="..." />
<meta property="twitter:description" content="..." />
```
- If I were to start over, the process simply is:
1. start ideating, sketching, and playing with [the og playground](https://og-playground.vercel.app) until you have struck inspiration and want to start migrating to your actual code.
2. create either a new Next.js project or add the og endpoint to your existing project.
3. modify the meta tags of your site to include the dynamic url. e.g. `https://www.dappling.network/api/og?type=project&id=${projectId}`.
4. test with the tools above
5. cross your fingers

If you're interested in our exact implementation, the full code can be found in [this gist](https://gist.github.com/Namaskar-1F64F/f499c85c286eaae6b2e6a7854539c140).
## The Art of the Web

Meta image generation is both a technical task and an art form. How one can translate the complex into the simple while maintaining beauty. Like a haiku captures a moment, a well-crafted meta image reminds that even in the technical world of web development, there's room for poetry, beauty, and a touch of the metaphysical. 

Thank you for your time. If you need help, please reach out to me [on telegram](https://t.me/folded_hands). 

🙏❤️🌱 What a great day to be alive!