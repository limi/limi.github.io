---
layout: post
hidden: true

redirect_from: "/articles/resource-packages/"

title: "Making browsers faster: Resource Packages"
subtitle: "A proposal to make downloading web page resources faster in all browsers"
cover_image: mountains.jpg

excerpt: "A proposal to make downloading web page resources faster in all browsers"

---

Update, summer 2011: The Firefox team has decided to pursue these types of improvements via SPDY and HTTP Pipelining instead, but I will keep this original article about Resource Packages online for historical purposes.

* * *

What if there was a backwards compatible way to transfer all of the resources that are used on every single page in your site — CSS, JS, images, anything else — in a single HTTP request at the start of the first visit to the page? This is what Resource Package support in browsers will let you do.

* * *

Update: There's now a proper [Resource Package specification], and any details there override whatever is written here. This article is a bit more conversational, and is useful to understand what we’re trying to do. If you’re looking for specifics — like what formats we support and how we parse — please refer to the formal specification instead of this article.

* * *

When it comes to browser performance, it’s widely known that a lot of the time is spent waiting for HTTP requests. You are probably familiar with the issue; a well-known optimization technique is to reduce the number of HTTP requests that are done for a given web site, since browsers only do 2–6 requests in parallel. This is why techniques like [image spriting] exist.

There are problems with image spriting, though. In addition to [potentially severe memory penalties], they obfuscate the code — “What is this icon at 704px, exactly?” — and every time you add a new icon, you have to update the sprite file, which adds to the maintenance burden.

Some images can’t be sprited (think about YouTube, which easily serves up 40 JPEG thumbnails on a given page), and there’s also other resources like JavaScript & CSS, which — while possible to combine — at the very least need one file each. You can see how this quickly saturates the available parallel HTTP pipelines.

Even if bandwidth is getting better, and the internet is getting faster, latency are actually getting worse in a lot of cases. With mobile internet browsing, and to some extent US domestic cable internet and DSL, the round-trip time for a single request can be slow, even if it downloads relatively fast once the transfer starts.

While there are lots of workarounds to solve this class of problems, we suggest a standard approach that all browsers makers can easily implement, and that is *backwards compatible* with browsers that do not support it. We also want a solution that works for all types of resources, not only image bitmaps, and one that doesn’t require any new tools, file types or protocols.

## Goals

This proposal has the following goals:

*   Make it possible to serve all the resources (images, stylesheets, javascript) required by a page in a single HTTP request, freeing up the other parallel requests to fetch resources that are page-specific.
*   Be as simple to implement as possible, so anyone with a passing familiarity with HTML should be able to perform the optimization.
*   Be entirely transparent to browsers that do not support it.
*   Avoid retransmission of existing resources.
*   Use existing tools that are widely used on all platforms.
*   Support the “80% use case” over adding a lot of complexity to the spec.

## Non-goals

Some explicit non-goals of this proposal:

*   Invent new file formats
*   Invent new compression formats

## Implementation

Our goal is to be compression-format independent, but the obvious candidates for support are:

*   `tar.gz`, aka. `.tgz` files — MIME type: `application/x-tar-gz`
*   `ZIP` files — MIME type: `application/ZIP`

While both `ZIP` and `tar.gz` files do not have not the most elegant or efficient packing format out there, they have the following very desirable traits:

*   Easily available reference implementations.
*   Can be unpacked even in partial state — which means that we can stream the file, and put CSS and JavaScript first in the archive, and they will unpacked and made available before the entire file has been downloaded.
*   Excellent toolchain support, available on all major platforms, so it’s easy for web developers to use.

Other formats, like `bZIP` may be more efficient in terms of file size, but they aren’t streamable and can’t be unpacked as partial files — so they are considered unfit for this particular purpose.

We propose this markup to signal a ZIPped resource package:

```
<link rel="resource-package" 
      type="application/ZIP" 
      href="site-resources.zip" />
```

The default MIME type for a resource package will be `application/x-tar-gz`, and you can omit it in documents where it is valid, like in HTML5, where an equivalent would be:

```
<link rel="resource-package" 
      href="site-resources.tar.gz" />
```

This will tell the browser to download this file first, and use the resources contained in the file instead of the referenced images, style sheets and javascript files — or for that matter, any other file. Browsers should prefer the files in the resource package, and do individual requests for images that are not contained in the package.

A given browser will probably block downloading any resources until the lists of files that are available in resource packages have been accounted for — or there may be a way to do opportunistic requests or similar, we leave this up to the browser vendor unless there’s a compelling reason to specify how this should work.

Older browsers that do not support resource packages will simply ignore this tag, and fetch the files normally, with one HTTP request for each.

## Path handling

Paths will be rendered relative to where the resource package is located, so you can supply additional directories inside the resource package to mimic existing site structure.

The resource package is referenced as follows in a page somewhere on the site `www.example.com`:

```
<link rel="resource-package" 
      type="application/ZIP" 
      href="/static/site-resources.zip" />
```

The ZIP file has this internal structure:

```
javascript/jquery.js
css/reset.css
css/grid.css
css/main.css
images/save.png
images/info.png
```

In this example, the resolved path to the `main.css` file would be `http://www.example.com/static/css/main.css`. Notice how the path inside the resource package is added to the path where the actual file is located.

## File listing using inline HTML

The most efficient way to declare what’s in the resource package without blocking to wait for the first part of it to load, is to do it inline in the resource package `<link>` tag itself:

```
<link rel="resource-package" 
      type="application/ZIP" 
      href="/static/site-resources.zip"
      content="javascript/jquery.js;
               css/reset.css;
               css/grid.css;
               css/main.css;
               images/save.png;
               images/info.png" />
```

The only problem with the above is that it will break validation for existing specifications like HTML 4 and XHTML 1.x, so we also support an alternate syntax, (ab)using the `title` attribute to get the same result:

```
<link rel="resource-package" 
      type="application/ZIP" 
      href="/static/site-resources.zip"
      title="javascript/jquery.js;
             css/reset.css;
             css/grid.css;
             css/main.css;
             images/save.png;
             images/info.png" />
```

This makes the file work with the existing validators and older standards.

## Fallback

There should be no compatibility issues with old browsers, as they will just load the individual files instead of the resource package.

Browsers that don’t implement this will seem slow in comparison to other browsers. Luckily, it should be a simple addition to any of the modern browsers.

## File listing using a manifest file

You can also give the browser the ability to know what files are in the resource package file without reading the entire file first, by adding a *manifest* file that can contain this information. This file can be supplied as a separate file (useful if combining with Offline Resources), or as the first file in the file itself. Of course, this will be slower to get than defining the contents inline in the HTML, but can be easier and cleaner to implement, especially if you're using offline resource support already.

Example `manifest.txt` file:

```
javascript/jquery.js
styles/reset.css
styles/grid.css
styles/main.css
images/save.png
images/info.png
```

*   This file *must* be the first file in the archive.
*   This file *must* be named `manifest.txt` when supplied as part of the archive.

If this simple format looks familiar, that’s not a coincidence. Initially, we were looking at using either an XML or JSON format to specify this, but we believe it’s easier to add a couple of new abilities to the offline resource specification instead. When using resource packages with [Offline Resources] (which are also [part of the HTML5 spec]), we’d like it to be easy to extend the rules, so the offline manifest with resource package support could look like this:

```
CACHE: resources=/static/siteresources.zip
javascript/jquery.js
styles/reset.css
styles/grid.css
styles/main.css
images/save.png
images/info.png

\# The above section lists the files in siteresources.zip.
\# To start a new section, do:
CACHE:
images/outside-package.png
```

The only thing we’d need to add to the HTML5 offline spec is that it should ignore anything on the same line after `CACHE:` if it doesn’t know how to handle it. This means that you could potentially put the resource package definitions in your Offline Resources manifest — we would also support doing it the other way around, and put the Offline Resources manifest inside the resource package.

This isn’t a requirement for implementing the initial version of resource packages, however — but could be an easy way to add support for it to the offline resource specification. If there’s a better way to do it, let me know.

## Duplicate files and override behavior

If a resource is defined twice on the same path — e.g. using multiple resource-packages — the file defined last takes priority. This enables an application with offline mode to synchronize changes without downloading the entire resource package again.

For example, on the first sync, the browser sees a resource package like this:

```
<link rel="resource-package" href="/documents/offline-0.zip" />
```

`offline-0.zip` contains:

```
manifest.txt 
doc1.html 
doc2.html 
```

Offline the application would be able to access documents at `/documents/doc1.html` and `/documents/doc2.html`.

The user then performs another sync and the browser now sees:

```
<link rel="resource-package" href="/documents/offline-0.zip" />
<link rel="resource-package" href="/documents/offline-1.zip" />
```

`offline-1.zip` contains:

```
manifest.txt 
doc1.html 
doc3.html
```

`doc1.html` and `doc3.html` would now be read from the `offline-1.zip`, `doc2.html` would continue to be read from `offline-0.zip`.

Even for use outside of the offline applications space, it allows pages to easily override the site look and feel with section-specific images and styling. A page could serve up:

```
<link rel="resource-package" href="/static/main-theme.zip" />
<link rel="resource-package" href="/static/section-theme.zip" />
```

Resources in `section-theme.zip` would take precedence over the content in `main-theme.zip` — making it easy to do overrides without replacing the entire resource package.

## Two simple examples

It’s not hard to see where Resource Packages could be useful in existing sites, two obvious categories would be:

Supply the core layout & functionality of a site

Typically, you would ship over all the CSS, JS and images that are used on every page in the site. These could be cached quite aggressively, and even use [ETags] to invalidate the resource package when needed.

The thumbnail search result case

Consider a [typical YouTube search results] page. It contains 20-40 thumbnails of videos, and there’s no easy way to add all these images into an image sprite, since the long tail of search results would vary a lot. Resource Packages would let you build a ZIP of search result thumbnails on the fly, and ship them all over in one HTTP request. It would require some CPU power, but would be much faster for the end-user. This wouldn’t have to be cached, or could be cached on a per-search basis.

## Other approaches

There are several other approaches that could solve parts of this problem, but they all have issues with current browsers and graceful degradation and/or are trying to solve a slightly different problem:

HTTP pipelining

This is a more aggressive way of utilizing the HTTP keep-alive mode, but is not implemented correctly by all web servers. Proxies have a hard time with it, some browsers also do, so it’s not really working unless you want to be aggressive and/or whitelist/blacklist certain servers.

It also causes a “head-of-line blocking” issue, where if you request X and Y on the same connection and X is slow, then Y is slowed too.

Multipart MIME

Hard for integrators, requires special packing that isn’t trivial to do, and has poor browser support.

JAR files or anything using data: URLs

No reasonable fallback mode, as the file name is embedded in the href/src link, and browsers that doesn’t support it just won’t render it.

SPDY

While this effort from Google aims to make everything faster, it is largely orthogonal to what we’re trying to do with Resource Packages. It also requires you to retrofit both web browsers *and* web servers to make it work, which means it will take quite a while before this will be in common use. Resource Packages work without any changes to the web server software, and will work as soon as any browser supports it — with no adverse effects to the browsers that don’t.

## Additional notes

*   The ZIP format doesn’t have MIME type support, so this will have to be solved by the browser based on filename extensions or other heuristics. We don’t believe this to be a problem, since browsers already have to do this.
*   All the resources in the package will have the same headers (expiry, [ETags], etc.) as the resource package itself. If you need different expiry dates or other caching settings, you should specify multiple resource files with different cache headers.
*   You can specify a charset in the resource package definition. If unspecified, it is assumed that any non-binary files inside are UTF-8.

## Next steps

We have sent this out to the major browser vendors for feedback, and we will be implementing this in the next upcoming release of Firefox — which tentatively has the version number 3.7, but this may change.

## FAQ

Does zipping up multiple optimized PNGs or other files work with ZIP? Can it potentially increase file size or lead to a high unpacking CPU overhead?

ZIP automatically chooses the best of deflation or no compression. Images will usually not be compressed, since they already are, text files like CSS/JS will be. In general, CPU impact from unzipping is negligible, even on slow devices.

How does this affect mobile devices, which have limited CPUs?

More realistic concerns are cache ability and bandwidth — as well as the latency on mobile networks — and memory. A lot of mobile browsers only keeps things in the browser cache at all if the individual file is something like 20kB or less. For returning visitors, you suddenly need to download one large file again, instead of having multiple small files locally.

In general, mobile browsers clear their caches quite aggressively — although with the resource package spec, one would hope that they would implement more optimal handling and prioritize caching these, since they more likely to be valuable for browsing performance than another random image/CSS/JS file in a site.

How would Resource Packages work with CDNs?

There would be no special handling, these mirrors would just carry the resource package file like any other file they are supplying.

How would you manage priority in the resource package? It would be useful to decide which files get downloaded first.

The priority is managed by the order they are added to the resource package. If you want a specific order, it’s trivial to specify this on the command line, so we aren’t adding any special syntax for this. Also, we have to do it this way to take advantages of the ability to unpack and display resources while the file is still downloading.

I worry about reduced parallelism. Lots of sites make heavy use of resource sharding across many hostnames to take advantage of multiple connections. Won’t this be a problem?

Sites usually use multiple hostnames to get around per-host connection limits, which are almost entirely a latency issue — not a bandwidth issue. Resource packages make multiple hostnames unnecessary, because it solves the latency issue a different way.

## Acknowledgments

*   [Mike Solomon] from YouTube for encouraging me to propose a solution to this issue.
*   [David Baron] & [Elika Etemad] from Mozilla for comments on the implementation feasibility, and for helping identify prior art.
*   [Vladimir Vukićević] & [Jonas Sicking] from Mozilla for help with adapting the Offline Resources standard to handle Resource Packages.
*   [Dion Almaer] & [Ben Galbraith] from Palm, [Steve Souders], [Gregg Tavares] & [Alex Russell] from Google & the Chrome team for feedback on the proposal from an implementer’s perspective.
*   [Ben Mathews] from Facebook for feedback on compression formats.
*   [Laurence Rowe] from [Jarn] for suggestions on how to handle duplicates/overrides.

## Improvements & feedback

If you have any suggestions on how to improve this proposal, comment in the open thread over at [Mozilla’s dev.platform forum]. It has been filed as [bug #529208] in Bugzilla for those of you that want to monitor its progress.

* * *

Proposal State: Ready for prototype implementation

Revision 7: Feb 22nd, 2010 — Added new information on compression formats, both `ZIP` and `tar.gz` files are now part of the spec.

Revision 6: Feb 12th, 2010 — Added a defined behavior for what happens when resources are defined twice.

Revision 5: January 10th, 2010 — added inline definition of resource package content in a manner that is compatible with HTML4/XHTML validators.

Revision 4: Nov 16th, 2009 — first published & widely circulated version, added Offline Resources support

Revision 3: Nov 10th, 2009

Revision 2: Sep 1st, 2009

Revision 1: Jun 15th, 2009

*[AMO]: addons.mozilla.org
*[CDNs]: Content Delivery Networks
*[CPUs]: Central Processing Units
*[CPU]: Central Processing Unit
*[CSS]: Cascading Style Sheets
*[DSL]: Digital Subscriber Line
*[HTML5]: HyperText Markup Language 5
*[HTML]: HyperText Markup Language
*[HTTP]: HyperText Transfer Protocol
*[IBM]: International Business Machines
*[JAR]: Java Archive
*[JPEG]: Joint Photographic Expert Group
*[JS]: JavaScript
*[MIME]: Multipurpose Internet Mail Extensions
*[NTLM]: NT LAN Manager
*[OS X]: Mac OS Ten
*[PDF]: Portable Document Format
*[SPDY]: Speedy
*[SSL]: Secure Sockets Layer
*[TLS]: Transport Layer Security
*[UI]: User Interface
*[URLs]: Uniform Resource Locators
*[URL]: Uniform Resource Locator
*[USA]: United States of America
*[US]: United States
*[UTF-8]: Unicode Transformation Format 8
*[VPN]: Virtual Private Network
*[XHTML]: Extended HyperText Markup Language
*[ZIP]: ZIP


[Resource Package specification]: http://people.mozilla.com/~jlebar/respkg/
[image spriting]: http://www.alistapart.com/articles/sprites
[potentially severe memory penalties]: http://blog.vlad1.com/2009/06/22/to-sprite-or-not-to-sprite/
[Offline Resources]: https://developer.mozilla.org/En/Offline_resources_in_Firefox
[part of the HTML5 spec]: http://www.whatwg.org/specs/web-apps/current-work/multipage/offline.html
[ETags]: http://en.wikipedia.org/wiki/HTTP_ETag
[typical YouTube search results]: http://www.youtube.com/results?search_query=ninja+cat
[ETags]: http://en.wikipedia.org/wiki/HTTP_ETag
[Mike Solomon]: http://www.culater.net/
[David Baron]: http://dbaron.org/
[Elika Etemad]: http://fantasai.inkedblade.net
[Vladimir Vukićević]: http://blog.vlad1.com
[Jonas Sicking]: http://sicking.cc/
[Dion Almaer]: http://almaer.com
[Ben Galbraith]: http://benzilla.galbraiths.org/
[Steve Souders]: http://stevesouders.com
[Gregg Tavares]: http://greggman.com/
[Alex Russell]: http://alex.dojotoolkit.org/
[Ben Mathews]: http://www.benmathews.net/
[Laurence Rowe]: http://objectvibe.net/blog
[Jarn]: http://www.jarn.com/
[limi@mozilla.com]: mailto:limi@mozilla.com
[Mozilla’s dev.platform forum]: http://groups.google.com/group/mozilla.dev.platform/browse_thread/thread/31779262c6b05205
[bug #529208]: https://bugzilla.mozilla.org/show_bug.cgi?id=529208