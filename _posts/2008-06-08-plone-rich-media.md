---
layout: post

redirect_from: "/articles/improve-plones-handling-of-rich-media"

title: "Improve Plone’s Handling of Rich Media"
subtitle: "Part 2: Making it easy to work with audio, video and other opaque content in the editing interface."
cover_image: mountains.jpg

excerpt: "Part 2: Making it easy to work with audio, video and other opaque content in the editing interface."

---

<span>If you haven’t</span> read [the introduction] and [part 1] yet, please make sure you do so before continuing. Thanks!

## A little bit of history

The background for this approach comes from a concept Geir Bækholt, Martijn Pieters and others at [Jarn] came up with to help [Trolltech] support a seemingly simple use case: They had a centralized price list, and wanted to make sure that everything referencing price was always up to date when they were changed the numbers in a central location. They were using code templates do do this, which is not such a good idea, since it effectively removes things from the indexed content, as well as making things fragile by mixing programming logic and content. Predictably, none of the content editors in the site were able to keep track of this.

To solve the problem, they invented something they called “snippets” — small pieces of text that had a special class that made them replaceable by a transform-on-save. I’m not going to cover this implementation in detail, but the outcome is that they realized that this approach could support a wide range of dynamic insertion capabilities relevant to audio/video media and other “opaque” content formats in Plone.

Where this gets interesting is when we combine it with a couple of other concepts, namely arbitrary attachments and portlets.

## Infrastructure: Arbitrary attachments and layout

Plone’s current notion of adding an Image object just to get an image into a page is a bit cumbersome. There are times when having a separate (i.e. end-user selectable) type for images makes sense, for instance in the “photo album” use case — but in the vast majority of cases, the user just wants to upload an image to use in a page.

In some cases, it may be desirable to use a centralized image repository, some of which may be pre-populated, but page editing is still the primary point at which the author considers images. What actually happens to the image when it is added isn’t really a concern to the user, and shouldn’t factor into how the UI works, which it currently does.

This problem is not specific to images. Adding a Flash animation or audio/video media files is no easier — quite contrary. From their experience with word processors and other desktop applications, the user expects to be able to embed files and resources directly into a document. Plone should make this as easy as possible.

The problem can be solved in a general, flexible way that makes it easy to create such attachments and lay them out on a standard page. The solution comes in three parts: widgets, editor integration and transformations.

## #1: Widgets

A new type of “contextual portlet manager” is added that can keep track of portlet assignments annotated onto a content item. Unlike the existing portlet managers for the regular portlet columns, this portlet manager is never actually rendered directly, but it acts as a container for embedded portlets. Importantly, the new portlet manager has a particular marker interface so that specific portlet renderers can be used when a portlet is assigned to this particular type of portlet manager.

We call these “widgets”. New widgets are defined that support attachment behavior. These must have a view that can render it as a simple icon as well as a canonical rendering. This can be achieved by having different portlet renderers registered for different marker interfaces on the portlet manager and/or request.

Portlets of this type could include an image widget and a media object widget. Both of these would include UI to upload resources directly into the widget and way to traverse to the uploaded image or file.

Additionally, they can consult a global option that lets the user choose a folder to act a repository for all images or other assets, via a pluggable storage policy. This makes it possible to have a central image repository, even though (to the end user) it looks like he’s just adding images to his page.

## #2: Visual editor integration

Kupu (or whatever visual editor we use as our core editor) gains the capability to manage such widgets directly. An add form that includes upload widget for the attachment widget may be shown directly inside Kupu. This can be made general enough so that other types of attachment widgets can show their own forms, although it is likely to require specific UI for images and media objects that use attachment widgets behind the scenes.

The important part: A widget is inserted into the body text by using an `<img />` tag that renders the widget’s icon representation. For image attachments, this may be an actual full-size or thumbnail representation of the image, for Flash, it might be a more abstract representation like just an icon. It also includes the widget id stored in an HTML class or similar.

The reason we use an image for this representation is that it’s the easiest element to manipulate in an in-browser editor like Kupu. The user can drag the icon around the page however he wants, and on saving the page, the transformation layer takes care of converting the placeholders into the real widgets. If you try to use other HTML tag constructs for this, you quickly run into visual editor issues specific to the various browsers.

## #3: Transformation

When the page is saved, a text transform looks for these special images and replaces them with the rendered representation of the relevant widget.

It should also be possible to optionally include attached items in the `SearchableText` of a content item, so it will return as a result if you search for a term included in the attachment.

## Turning portlets into widgets, a note on terminology

The idea of “widgets” needs some more explanation, as well as some implementation-specific notes:

To make portlets more understandable and generic, we rename them to “widgets”. The implementation still refers to portlets, this is a UI change only. There are some behavioral changes that would make widgets more generally useful.

The main reason for the renaming is that “portlet” is very “jargon”-y — your mom certainly does not know what a portlet is. To add insult to injury, the Plone portlets implementation isn’t really what the rest of the world is talking about when they are referring to portlets — they are usually talking about the [elephantine JSR-168 Java specification]1, so let’s eliminate that source of confusion once and for all. 1To be fair to Plone, the JSR spec was still in its infancy when this particular part of Plone was named.

“Widget” is the term used by several web-based and desktop-based implementations of this concept, and people understand what they are at this point. “Gadgets” is another term that might be an alternative, but for the remainder of this proposal, I will use the term “widgets”.

On a related note, the intent is also to get rid of the notion of left and right columns (as well as the general separation that forces you into a certain layout at the moment), but I’ll save that layout discussion for a later point. It’s covered superficially in the technical explanation at the end of this part, if you’re interested. The short version is that everything becomes a widget (the search box, the navigation, breadcrumbs, logo, etc).

## Demonstration <abbr title="and">&amp;</abbr> Notes

<video src="/media/simplify-edit-ui.mov" title="Movie showing the new way to handle rich media" autoplay loop></video>

*   Insert a Movie widget
*   It allows you to `Upload`, `Search/Browse` an existing content object, or `Embed` from an external web site, potentially with some settings related to height/width, etc.
*   For movies and audio, an add-on product suite like a stripped-down version of Plone4Artists could supply transcoding and metadata extraction for these types. Simple metadata extractors like EXIF (photo metadata) and ID3 (mp3 metadata) could be in the core, but we don’t really want to ship a full suite of video and audio converters by default.
*   Right-clicking a widget and/or possibly clicking an information icon brings up the widget settings.

As you can see, this solves a lot of interesting use cases, and the potential new capabilities here can not be understated, it’s an incredibly flexible and powerful way of handling “opaque” content.

If you’re interested in the technical implementation details, continue reading — if not, you can jump directly to [Part 3: Composite Pages, Listings & Content Proxies].

## Implementation notes

Some implementation specifics from Martin:

*   Some widgets are global. They are shown on all pages. This is *not* the same as assigning a widget at the portal root that is acquired everywhere. One use case here is things like promotions or site-wide information, but this mechanism can also be used to manipulate site layout (see below).
*   There is a “Layout and widgets” control panel were all global widget assignments are managed. This includes site-global, group-assigned and type-assigned widgets.
*   Global categories of widgets (assigned by group, content type or globally) can be blocked by location as they are now. This should show a visual representation of what gets blocked. It should be possible to block all widgets in the given category, or only some.
*   When a contextual widget is added (i.e. one that is attached to a content item), it is for that exact content item only by default.
*   A particular widget assigned to a folder may optionally be set to apply to items in the folder only, or to items in the folder and its subfolders. However, acquisition is not the default.
*   Widget assigned to default pages behave as if they’re assigned to the relevant folder.
*   A portlet manager can be associated with a layout grid. A widget’s position in the grid is annotated onto the widget’s assignment instance. This alleviates the need for multiple portlet managers that only serve to represent different areas of the screen.
*   For this to work, there must be a more granular way of asking a portlet manager to render itself than the current approach which uses the `structure:` expression type in TAL. Most likely, this will just be a view with a function to render a particular grid cell of a particular portlet manager.
*   Some widgets can only be added as site globals, never to groups, content types or content items. These are layout elements that fit into a global grid that includes the entire page.
*   A convenience wrapper widget type makes it possible to wrap any supported viewlet into a widget. “Supported” here means that the viewlet provides a particular marker interface.
*   By moving viewlet widgets around the site-global grid, it is possible to move the search box, say, from the top right hand corner to anywhere else on the screen.

With that out of the way, it’s time to move on to [Part 3: Composite Pages, Listings & Content Proxies].

[the introduction]: /simplifying-plone
[part 1]: /plone-editing
[Jarn]: http://www.jarn.com
[Trolltech]: http://www.trolltech.com
[elephantine JSR-168 Java specification]: http://jcp.org/aboutJava/communityprocess/review/jsr168/
[Part 3: Composite Pages, Listings & Content Proxies]: /composite-pages
