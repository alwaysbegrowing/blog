---
title: Hosting Static Sites on IPFS
description: It's like, the same, but different.
date: 2023-06-04
tags:
  - ipfs
  - tooling
  - learning
  - frontend
image:
  src: /images/hosting-static-sites-on-ipfs/chrome-dev-tools-errors.png
  alt: chrome dev tools with errors
---

Well, I’ll start that IPFS isn’t special. It’s cool, yeah, but the problems faced when deploying sites to IPFS, discounting the potential loading time, and pinning issues, appear in any hosting environment. The root cause lies in how the resources to produce the webpage are accessed. When accessing a site through IPFS, one can think of it as a simple folder in the sense of a folder on one’s computer. 

## Absolute Paths - /some/examples

### Local File System Example

Say you want to view your poker hand calculator web app. This existed on the web before, but you being the pragmatic data-saver, downloaded a local copy to your computer. The site was open source, after all. It’d be wrong to ****not**** keep an extra copy. Now how do you view it? 

1. Search for the folder. It might be less successful on Windows. 
    a. Realize you don’t remember the name and manually traverse your download folder. You know it’s in there somewhere.
    b. Ahh, there it is.
2. Open up the index.html page. Notice in the supposed “web” browser, the file appears as a weird-looking URL, or is it a URI…


![local path example in a white box](/images/hosting-static-sites-on-ipfs/local-path-example.png)

    

And observe the page is… blank?


![empty webpage with browser bar](/images/hosting-static-sites-on-ipfs/empty-webpage-browser.png)



Well, yes, but that isn’t to say your file system didn’t try. You can see the requests made, and it’s a sea of red.


![chrome dev tools with errors](/images/hosting-static-sites-on-ipfs/chrome-dev-tools-with-errors.png)



Why? Well, let’s look closer at a request.


![an example request in chrome dev tools](/images/hosting-static-sites-on-ipfs/an-example-request.png)


The requested URL looks surprisingly even stranger. 

```bash
Request URL: file:///_next/static/css/d22f27d70fc00611.css
```

So where did all of the rest of the URL go? Clearly, in the top bar, you can see 

```bash
file:///Users/shire/downloads/cool-websites/poker-calc/index.html
```

The answer lies in how relative and absolute URLs request resources. See, inside the index.html file, our CSS file is requested with a beginning `/`.  This instructs the browser to request a resource with an implicit domain name. [Read more on MDN](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL#absolute_urls_vs._relative_urls). So great, the `CSS` file is being requested with an implicit domain name.

```html
<html>
  <head lang="en">
    <meta charset="utf-8" />
    <link
      rel="preload"
      href="/_next/static/css/d22f27d70fc00611.css"
      as="style"
    />
	</head>
</html>
```

Because there’s no domain name, the browser assumes the resource is at the root of my computer’s hard drive. You fool!

### This does not work! Failure.

## Domain Hosted Example

So assume our poker app also exists at https://pokercalc.app. When we visit the site, we implicitly request the `index.html`. That is returned, and, what? Everything looks good. The requests are going through. You look through the network requests again, and they look, normal:

```html
Request URL: https://pokercalc.app/_next/static/css/d22f27d70fc00611.css
```

Ahh well, here’s where the implicit domain comes in with absolute URLs. The `[pokercalc.app](http://pokercalc.app)` has been implicitly added to the request and since the resource is also on the web server, the file is returned.

### This works! Success.

## Sub-Path Hosted Example

So what’s the big deal? Well, now assume the content, instead of being on `https://pokercalc.app`, has a summer theme hosted on `https://pokercalc.app/summer`. The same files will be requested, and the same requests again will be returned.

Even though clearly our top bar shows the correct website

```html
https://pokercalc.app/summer
```

The request seems to have chopped off the summer path.

```html
Request URL: https://pokercalc.app/_next/static/css/d22f27d70fc00611.css
```

Yet another victim of the implicit domain.

### This does not work! Failure.

## Relative Paths - More/examples

To save the day, we have relative paths. Again, [more information on MDN](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL#absolute_urls_vs._relative_urls). Walking through the same examples again, say we changed the HTML thusly. 

```diff
<html>
  <head lang="en">
    <meta charset="utf-8" />
    <link
      rel="preload"
-     href="/_next/static/css/d22f27d70fc00611.css"
+     href="_next/static/css/d22f27d70fc00611.css"
      as="style"
    />
	</head>
</html>
```

### Local File System

`file:///Users/shire/downloads/cool-websites/poker-calc/_next/static/css/d22f27d70fc00611.css`

### Domain

`https://pokercalc.app/_next/static/css/d22f27d70fc00611.css`

### Path

`https://pokercalc.app/summer/_next/static/css/d22f27d70fc00611.css`

And all of those requests are actually successful. Hooray!

### These all work! Success.

## IPFS

Remember how I said IPFS isn’t much different than a simple folder? Well, it’s a bit more complicated than that. See sites accessed on IPFS can be accessed via their Content Identifier ([CID](https://docs.ipfs.tech/concepts/content-addressing/)) in different ways. 

Native protocol: `ipfs://<CID>`

Web gateway: `https://ipfs.io/ipfs/<CID>`

IPNS (web gateway): `https://ipfs.io/ipns/<domain name>` 

Behind our CDN: `https://preview-domain.dappling.network`

And while all of these would appear to work with relative URLs, there is a catch.

## The catch…

So we can’t just throw all of the URLs to relative and call it a day. Well, you can actually get close, and we did just that with a script aptly called `replacePaths`.  Here are the replacements being made in the source code of the websites, and the full code can be found at this [gist](https://gist.github.com/Namaskar-1F64F/6515b349e596bd712e4593a4d9a345c9).

```jsx
const replaceRules = [
  // Replace all absolute paths with relative paths
  // Replace src="/example" with src="example" and href="/example" with href="example"
  // Replace url("/example") with url("example")
  // Replace src="/example" with src="example"
  // Replace href=\"\/example\" with href=\"example\"
  // Replace href: \"\/example\" with href: \"example\"
  // Replace href: "/example" with href: "example" and path: "/example" with path: "example"
  // Replace all /_next with _next
]
```

How could this possibly fail? Well, besides modifying the source files with some basic string replacements and hoping for the best, there exists some nuance in NextJs and React routing.

### Routing in React

I specifically looked into the NextJs router for this, but the React router should behave similarly. For every link on the page that is clickable, there is a destination. These again are going to be relative or absolute URLs and note that fully qualified domains like `https://example.com` are also considered absolute URLs. There are also local browser states that are handled with functions like `Router.push` which change the page content AND modify the URL in the top bar. Lucky for us, these are somewhat easy to think about as they follow the same patterns of relative and absolute URLs. 

For example, if we are on the page `[pokercalc.app/texas-holdem](http://pokercalc.app/texas-holdem)` and we have a button that will `push` the state to `/games`, the browser will happily change the bar to `pokercalc.app/games`. Good, right? 

Wrong!

What if we were hosting this on a different path, say `pokercalc.app/summer/texas-holdem`, and then tried pushing `/games`? Well since the URL is clearly “absolute”, we get what we expect: `pokercalc.app/texas-holdem`. No summer theme is to be found. The implicit domain strikes again.

But what about `basePath`?

### `basePath`

Good though, myself from a second ago. The NextJs config option [basePath](https://nextjs.org/docs/app/api-reference/next-config-js/basePath) is made specifically for this. It’ll prepend all of the links with the specified `basePath`. Hooray!

but wait…

Remember how we don’t know *****where***** our website will be accessed? How are we supposed to choose our `basePath` at build time? Well, you simply cannot.

There’s a GitHub repo with these things that I failed to find, but the basic idea is to change the code to make the site work wherever it is hosted.

1. inject a `base` into the document which will set the actual URL to a node on the document, accessible throughout the app.
    
    ```jsx
    const scriptTxt = '
    (function () {
      const { pathname } = window.location
      const ipfsMatch = /.*\\b(Qm[1-9A-HJ-NP-Za-km-z]{44,}|b[A-Za-z2-7]{58,}|B[A-Z2-7]{58,}|z[1-9A-HJ-NP-Za-km-z]{48,}|F[0-9A-F]{50,})\\//.exec(pathname)
      const base = document.createElement('base')
    
      base.href = ipfsMatch ? ipfsMatch[0] : '/'
      document.head.append(base)
    })();
    ';
    // .. 
    <Head lang="en">
      <script dangerouslySetInnerHTML=\{\{ __html: scriptTxt \}\} />
    </Head>
    ```
    
    Now I don’t know if this is simply a bad way to pass around a variable or if NextJs does something internal with it.
    
2. Replace all links with a new component that actually uses the link:
    
    ```jsx
    const BaseLink = ({ href, as, ...rest }: LinkProps & BaseLinkProps) => {
      console.log("BaseLink", href, as);
      const [newAs, setNewAs] = useState(as || href);
    
      useEffect(() => {
        if (newAs.startsWith("/")) {
          let baseURI_as = "." + href;
    
          if (typeof document !== "undefined") {
            const url = new URL(href, document.baseURI);
            baseURI_as = url.href;
          }
    
          setNewAs(baseURI_as);
        }
      }, [as, href, newAs]);
    
      return <Link {...rest} href={href} as={newAs} />;
    };
    ```
    
3. Change how the Router pushes as well. There is a possible solution that admittedly I didn’t follow to fruition. I stopped because I think the internal routing table, I believe Next has a way to map these routes to the javascript to load, but altering that file does not seem to be the approach. This is what I tried anyway, but I think I’m misunderstanding how routing works.
    
    ```jsx
    export function basePathPush(path: string, as: string = path, options?: any) {
      let newAs = as;
      if (!newAs.startsWith("http")) {
        newAs = "." + newAs;
        if (typeof document !== "undefined") {
          console.log("basePathPush newAs.startsWith document", path, as);
          newAs = new URL(newAs, document.baseURI).toString();
        }
      }
    
      return Router.push(path, newAs, options);
    }
    ```
    

## A Table?

|  | Absolute Paths | Relative Paths | NextJs Routing |
| --- | --- | --- | --- |
| ipfs://<cid> | Maybe. Haven’t tested recently | Works for assets | Maybe |
| https://ipfs.io/ipfs/<CID> | Definitely not | Works for assets | Works for some navigation. |
| https://ipfs.io/ipns/<domain name>  | Definitely not | Works for assets | Works for some navigation. |
| https://preview-domain.dappling.network | Definitely | Definitely | Definitely |

So, clearly, the CDN version works fine, but we should have already known that. We’re hosting a static website through a domain. Absolute paths work how they should, and relative paths work how they were designed. I say designed because when developers are developing there is an implicit assumption of this running from a domain. 

## Examples

Just to round it out, here are some examples to show what I was telling up there.

### The site doesn’t work

There was `useEffect` on the first load that would cause the app to start off in a bad state.

```jsx
const FixedYield: NextPage = () => {
  useEffect(() => {
    Router.push("/smart-yield/pools/");
  }, []);
```

[https://cloudflare-ipfs.com/ipfs/bafybeiab75ag4g7qgdhfkkx454y72ubkrgd7jadhk4ipnndkbtpa2cluoy/](https://cloudflare-ipfs.com/ipfs/bafybeiab75ag4g7qgdhfkkx454y72ubkrgd7jadhk4ipnndkbtpa2cluoy/)

### Absolute pathing

[https://cloudflare-ipfs.com/ipfs/bafybeihje65wpty2ecibevbc7o3nwu7vnbhdn6xua5d5p4ni44fipn6ppe/](https://cloudflare-ipfs.com/ipfs/bafybeihje65wpty2ecibevbc7o3nwu7vnbhdn6xua5d5p4ni44fipn6ppe/)

Here’s an example that has absolute paths. 404s galore.


![chrome dev console errors](/images/hosting-static-sites-on-ipfs/chrome-dev-tools-errors.png)



## Relative pathing

[https://cloudflare-ipfs.com/ipfs/bafybeighrlepp5pdmdcxir3zpwl2cjmdb6jma5danb3ex7i76ornmn6mbu/](https://cloudflare-ipfs.com/ipfs/bafybeighrlepp5pdmdcxir3zpwl2cjmdb6jma5danb3ex7i76ornmn6mbu/)

Here’s an example that replaces absolute paths with relative ones. You’ll see the sub-nav works, but re-writes the URL. The top nav doesn’t work. Not sure why.

### Changes

Here is a full preview of my changes. Still doesn’t quite hit the mark but might be a good place to start. Might need access from me to see.

[https://github.com/Namaskar-1F64F/barnbridge-v2-frontend/commit/9770763297375d0f6f37f569d9a5679b47a53b55](https://github.com/Namaskar-1F64F/barnbridge-v2-frontend/commit/9770763297375d0f6f37f569d9a5679b47a53b55)

## What did we learn?

I don’t know about you, but I learned IPFS can be thought of as a website accessed on a path on a domain. One needs to consider that the URL will not be known at build time and it seems like the best option right now is just simply shoving the whole situation into a familiar box. By using a domain and serving the IPFS content behind our CDN, we make the websites as the user expects.
