---
layout: post

redirect_from: "/articles/composite-pages-listings-and-content-proxies"

title: "Composite Pages, Listings & Content Proxies"
subtitle: "Part 3: Making it easy to do advanced page layout tasks in Plone"
cover_image: mountains.jpg

excerpt: "Part 3: Making it easy to do advanced page layout tasks in Plone"

---

If you haven’t read [the introduction], [part 1] and [part 2] yet, please make sure you do so before continuing. Thanks!

…and now for the [pièce de résistance]:

It’s time to reduce the number of concepts and approaches in Plone. Now that we have the “widget” concept and the new editor capabilities, we have the tools needed to:

*   Eliminate redundant types,
*   Eliminate the need for a separate “Composite Page” type,
*   Eliminate the need for a “Display” menu,
*   Eliminate the need for a separate “Content Proxy” type,
*   Eliminate the “Default Page” concept,
*   Eliminate the need for a specialized “Folder” type.

I’d also like to get rid of the “Contents” tab, but let’s keep this out of this particular proposal for the time being, since that’s a separate discussion. Ideally, there should be a separate “batch operations” interface for batch edit/rename/move/workflow changes. This proposal can be implemented independently, however — and indeed there is currently a Summer of Code project called [plone.app.batch] this year, working on some of this functionality.

## Eliminating redundant types

*   “Image” as a content type usually doesn’t make sense in itself. One exception here is the “photo album” use case, but that’s usually not what we refer to when talking about images in Plone. By making the images work the way content editors expect them to, we reduce the complexity of creating a page with Plone, and in one fell swoop get rid of the need for add-ons like RichDocument, PloneArticle and a lot of other generalized "keep the resources with the content" products.
*   “Collection” (aka. Smart Folder) does really not make any sense as a content type by itself either. It’s a stored search, meant to be used in a context. By implementing it as a widget instead, we free up the content editor to use it in more flexible ways.

## Eliminating the need for a separate “Composite Page” type

Another way of thinking about the widgets system is that *every* page is essentially turned into a page with composition abilities similar to the existing third-party “composite page” types.

Thus, every page is a composite page, and can pull in resources from other locations — folder listings, searches, images, movies, forms — you name it.

## Eliminating the need for a dedicated “Display” menu

The reason why the Display menu exists in the first place is to make it possible to have a specialized view of a folder. But usually, the listing in itself only gives you half of what you want — you may want a small blurb talking about what the listing contains, an illustration or even a demo movie to go along with the listing.

What is currently thought of as “folder listings” are really just a special case of a “listing” widget — plain folder listing or a stored search.

## Eliminate the need for a “Content Proxy” type

By introducing a “Content Proxy” widget — and yes, I’m sure we can find a better name for it — one can construct a page composed of any other number of pages.

From the simple case of mirroring one page in a different location to the more complex case of, say, constructing a “manual for Plone integrators” composed of 15 FAQs, 30 how-tos and 5 tutorials that all live in separate locations.

Another way to look at it is that to create a content proxy, you create a page with a single widget that pulls its content from somewhere else — and this content can even potentially be external. Hosted in [Google Docs]? No problem!

## Eliminate the “Default Page” concept

After all this, you can probably guess how we can get rid of the “Default Page” concept. Yes, by using widgets and the new way of doing layout, on a folder — or a page, if we get rid of the “Folder” type as outlined below. The composite page abilities in the new approach will be able to turn a folder/page into any type of listing — even a mirror of an existing page.

Technically speaking, if you look at the folder from a file system view like WebDAV or FTP, its “body text” will look like a folder with an `index` document inside, similar to how `index.html` works for HTML files.

## Eliminate the need for a dedicated “Folder” type

This a suggestion where I’m still not 100% sure we can make it happen without any performance implications or other problems, but ideally I’d like to get rid of the “Folder” concept altogether. Currently, folders are used to create listings of content added to a particular area — and that’s just as easily accomplished by adding a “dynamic listing” widget. It can list content based on containment, similar to what a “Folder” does today, or based on a set of criteria, like the “Collections” do.

To me, it makes sense that all objects are “folderish” — to use the Zope jargon for this — but I’m sure there are some constraints and considerations that need to be discussed. This is by no means a necessary part of this proposal, but it *would* make the experience a lot simpler for the end-user if we could make it work that way. Hopefully the new, unified [plone.folder] implementation scheduled for the next major release of Plone will make this easier too.

[Let's summarize what we have outlined so far].

[the introduction]: /simplifying-plone
[part 1]: /plone-editing
[part 2]: /plone-rich-media
[pièce de résistance]: http://en.wikipedia.org/wiki/Pièce_de_résistance
[plone.app.batch]: http://plone.org/products/plone-app-batch/
[Google Docs]: http://docs.google.com
[plone.folder]: http://plone.org/products/plone/roadmap/191
[Let's summarize what we have outlined so far]: /simplifying-plone-conclusion
