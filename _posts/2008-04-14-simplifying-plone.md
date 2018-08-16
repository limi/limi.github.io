---
layout: post

redirect_from: "/articles/simplifying-plone"

title: "Simplifying Plone"
subtitle: "It’s time to look ahead and see what we can do to make the Plone experience even better for the upcoming releases"
cover_image: mountains.jpg

excerpt: "It’s time to look ahead and see what we can do to make the Plone experience even better for the upcoming releases"

---

The [Strategic Planning Summit] at the Googleplex in February 2008 challenged us to focus on “approachability” as a major design goal; a way to make sure integrators who try to put together a project using Plone will have a good first experience. I won’t go into that in great detail, but I believe Martin Aspeli’s article [Pete and Andy try Plone 4] serves as a great primer for the kind of experience we’re reaching towards for our Integrator audience. If you haven't read it yet, please do.

But what about the people that will use those Plone deployments as their intranet, web site or collaboration space in the years to come? Don’t they deserve just as great an experience the first time they are introduced to Plone? Shouldn’t they be able to master the basics of Plone in about half an hour of tinkering, and be able to self-graduate to higher understanding of the system when they see the need for it?

Let’s skip the all rhetorical questions for which the answer is obviously a resounding “yes” — and let’s not forget that Plone was a revolution at the time, and enabled a lot of non-technical people to publish and work with content through their web browsers when few such solutions existed.

This was before Google Docs, Basecamp, mainstream wiki usage, and even before the word “blog” was something most people knew what was. Netscape 4 was still a viable browser, Windows 2000 was the new hotness, and Napster was teaching people the about the power of networks of people and media. Mac OS X and the iPod was just a glimmer in Steve Jobs’ eye, and [Clippy from Word was the most widespread alternative UI] for content authoring. People thought we were crazy to use CSS everywhere for layout, and a new project called Wikipedia asked if they could [use our style sheet and layout], and we said yes.

In order to to understand this proposal, an appreciation of what was done in Plone from 2001 and up until today — versions 1.0, 2.0, 2.1, 2.5 and 3.0 — is useful. Painted with a broad brush, all those releases were incremental — in a UI sense, that is. The infrastructure went through great improvements, but the user interface just adapted to fit around the new concepts that were introduced, and the core usability approach didn’t change much. Sure, Ajax functionality, a visual content editor and other nice improvements were added, but the basic way of doing content authoring remained.

There are good things to be said for interface stability, and I’m really not proposing major changes in how you interact with Plone on a day-to-day basis, since it generally works well. Changing a few key elements can have profound effects on the overall approachability for the average Plone end-user, however. We can also make it easier for the power users who create and manage complex content layouts and rely on rich media like movies and audio to make their sites more compelling and rewarding.

This new UI approach is gradual in its implementation — “progressive disclosure” for you UI designers out there — but revolutionary in the way it makes things both simpler and more efficient and powerful at the same time. Rather than trying to separate the users into explicit "simple user" and "power user" roles, we make it easier to reach the things you use all the time, and push some of the functionality to locations where people can find them without interfering with the general content authoring. So it’s similar to — but more implicit and gradual than — the idea of a dedicated Power User role.

Whether the elements of this proposal end up in Plone 4.0, Plone 5.0 or Plone 3000, I don’t know. What’s important is that it’s possible to implement pretty much all of these things as incremental improvements in the coming releases in the 3.x series, as they make sense as smaller improvements by themselves — or don’t need to be visible until we flick the switch in a later version.

## What can be improved?

Plone mostly works great, but there’s no reason to stop innovating! Some of the major areas I think should be improved — and are covered in this proposal — are:

*   Rich media handling and dealing with anything that is not a simple page of text is harder than we’d like it to be. We want it to be equally easy to write content that consists of images, movies, audio and content embedded from other web sites.
*   Composite pages — pages that pull content from a number of locations — is a hard concept to get right, usability-wise. None of the existing add-on products have supplied a satisfactory solution to this. No offense intended, of course — it’s a very hard problem, and people have done a great job exploring various different approaches to this.
*   The content authoring process is currently very artificial — you generally need to make sure your images, audio or movies are uploaded to the system before you start writing. This is the opposite of how people usually think and work.

## What are we proposing to do?

We suggest a set of changes that will make Plone easier to use, as well as reduce the complexity of (or getting rid of entirely!) the concepts of:

*   Collections
*   Images
*   Composite page types
*   Content Proxies (represent one object in two locations)
*   Default pages
*   “Contents” tab
*   Rich media
*   Content reuse

A tall order, for sure. Suspend your disbelief for a moment, and let me show you how! I know this can seem a bit overwhelming when taken in one sitting, but I want you to see the progression and thinking along the way — as well as the ideal end state.

## Preamble and credits

This proposal is comprehensive and wide-reaching, but it’s *not* a result of me putting on my thinking hat for three months, retreating to my secret mountain hideaway with an unlimited supply of my favorite whisky and music. Although that does sound tempting, in reality it’s a result of great conceptual ideas from people like Geir Bækholt, Martin Aspeli, Benjamin Saller, Helge Tesdal, Danny Bloemendaal, Cornelis Kolbach, Duncan Booth as well as my fellow UI designers at Google — you know who you are! As usual, my role is mostly putting all these ideas together into a coherent whole, and hopefully I give their ideas enough credit and appreciation by proposing something that is more valuable than the sum of its parts, clichés notwithstanding.

So to anyone who has been involved in this discussion over the past couple of years — too many to mention, so I won’t even try to name you all — thank you for your great input and suggestions! Also a very special thank you goes out to Martin, who wrote the initial summary of the infrastructural description on his way back to London based on the description Geir and myself gave him a few hours earlier at the Planning Summit.

## How to read this proposal

There are several ways to explain what this proposal covers, and Martin and Geir have helped me with the implementation specifications. If you’re mainly interested in how this will end up from the end-user point of view, I suggest that you read through the technical parts anyway — and ignore anything you don’t immediately understand. It will give you a fuller understanding of the screencast demonstrations and screenshots that follow.

I started out with two separate articles on how this would work, but realized that for our developers it’s all connected. In the interest of efficiency — and actually getting this published — you’ll have to live with the somewhat interleaved explanation for now.

It’s tempting to write a proposal that was five times as long trying to cover all the details that are touched on in this proposal, but then nobody would read it. So let me know what needs clarification either by [contacting me directly] or by posting to the [Plone Core Developer list], and I will explain in more depth — and if necessary update the proposal to clarify when things are confusing or non-obvious. Most of the edge cases — believe it or not — have been thought through. At least we think so, but you are of course welcome to show us new ones.

It’s time for a revolution, it’s time to make Plone’s user experience more powerful, as well as making it simpler to get started! But enough pillow talk, let’s get down and dirty with [Part 1: Simplify the Editing Experience].

[Strategic Planning Summit]: http://plone.org/events/2008-summit/
[Pete and Andy try Plone 4]: http://martinaspeli.net/articles/pete-and-andy-try-plone-4
[Clippy from Word was the most widespread alternative UI]: http://bash.org/?122557
[use our style sheet and layout]: http://en.wikipedia.org/skins-1.5/monobook/main.css
[contacting me directly]: /contact
[Plone Core Developer list]: http://www.nabble.com/Core-Developers-f6745.html
[Part 1: Simplify the Editing Experience]: plone-editing
