---
layout: post

redirect_from: "/articles/simplify-plones-editing-experience"

title: "Simplify Plone’s Edit Experience"
subtitle: "Part 1: Simplifying Plone’s content authoring experience for end-users"
cover_image: mountains.jpg

excerpt: "Part 1: Simplifying Plone’s content authoring experience for end-users"

---

Over the past few weeks, I have done some experiments in simplifying the UI to make Plone less intimidating for newbies. A lot of these experiences are based on deploying it in a limited fashion internally here at Google. Generally, Plone is in a state now where most of the major areas of functionality is in place, so it’s time to look at it with fresh eyes — it’s not exactly Plone 1.0 when it comes to number of things you can click anymore.

I have built a clickable prototype of the new UI, since the best way to show you what the new editing experience looks like is to show it in action.

Note that while I’m using the NuPlone theme in this demo, the approach works equally well with any theme — this is about functionality, not looks.

## Demo <abbr title="and">&amp;</abbr> Notes

<video src="/media/simplify-edit-ui.mov" title="Movie showing the new approach" autoplay loop></video>

***

* First we show the existing UI, and highlight the various features.
* Then we switch to the new proposed UI. The major difference is that interface usually only has one button — “Edit” — instead of the entire tabs/menu/button structure that was there in the earlier version.
* Adding content: Showing the simplified new add menu, notice how there’s fewer types. We’ll get to the rationale between these going away in the next parts.
* When we click the edit button, it does an async request and brings back the edit form inline. It turns out that making everything editable inline all the time is confusing users, and contributes significant amounts of visual noise.
* A number of interface elements and functionality has been moved to the edit screen. Sharing and History are now on the Edit screen, since they are usually part of the editing flow.
* Another common trait of these actions is that they are used only occasionally compared to the adding and editing of content.
* The “Advanced” menu handles everything that isn’t in the “Default” fieldset, i.e things like Effective/Expiry dates, ownership, copyrights, local workflows, etc. It also handles add-on screens that would sometimes show up as additional tabs in the old-style interface.
* Share, History and Advanced show up layered on top of the main content, lightbox-style when clicked — a modal window in UI terms. This reinforces that the metadata and functionality is a layer around the content. It’s not shown in the screencast, as it takes more time to fake than I have right now. Just imagine the existing fieldset implementation in a lightbox-style overlay, and you’ll get the idea.
* Moving on to the visual editor, we have collapsed down some less-commonly used buttons to a pull-down menu to make the interface as minimalist as possible. Not to worry, though: if you have certain things that deserve a dedicated button like — let’s say, heavy use of the “highlight” style — these can appear on the main toolbar too. Don’t let the super-minimalist button setup distract you at this point, it’s there to illustrate the concept.
* Most significant of the two menus is the “Insert” menu, which can insert a number of familiar and unfamiliar content components called “widgets”, we’ll get to these in part 2:
  *   Tables
  *   Widgets
  *   Listings — both folders and dynamic searches (Collections)
  *   Application functionality like Polls and Forms
* This is another one of the ideas that will Change Everything™, since there is no need for a dedicated Collection type anymore. If you want to mimic the current Collection behaviour, it would be a page with only the Collection widget in it. This is an example of how the widget idea starts simplifying everything in unexpected ways. I’ll cover this in more detail in part 2.
* Some things would be moved to the main edit screen, like tags and other elements deemed important enough to live there. Don’t pay too much attention to the UI on the tag field, it’s just an input box in this mock-up — of course it would have autocomplete in its final form.
* Workflow state has moved to the bottom of the form, as the natural action is to do some edits, and then submit the document as part of the editing flow. For people with the Reviewer role/permission, the idea is to add an additional pulldown on the view of the content too, so they don’t have to go to the edit screen to make state changes — in other words the same way as things work in the old-style UI, but limited to the power users that are reviewers.
* We show how you would submit the document for review, and Save it. The current thinking is that changing the state would show a review comment string field next to the pulldown, so you could enter an optional comment to go along with it.

## Frequently asked questions and additional notes

**What happens to in-line editing?**

If there’s something that we have heard loud and clear from our integrators, in-line editing — aka. “click to edit” — doesn’t work that well with end-users, so we’ll disable it for general content authoring. Yes, I just admitted we were wrong the first time around.

Instead of trying to work around the problem of editing efficiency with “UI hacks” like these, we should attack the root of the problem, which can be done in much the same way, but with fewer moving parts. There *are* use cases — mostly specialized applications — where inline editing may be the right approach. This proposal just states that for the basic content authoring use case, it doesn’t add much — except for confusion, annoyance and visual noise.

**Where is the current state shown on the view, since the menu bar is gone?**

The current plan is to fold it into the by-line along with the publishing date and author info.

**What happens to products that have defined custom tabs and/or menus?**

Ideally, there are very few legitimate use cases for doing this, but I agree that some exist. I won’t call out which are gratuitous and which make more sense, but I’ll use LinguaPlone — Plone’s multilingual support — as an example of where it might make sense to add a new menu.

*   Additional tabs end up as sheets in the “Advanced” edit page.
*   Menus end up as pulldowns in the view UI (but are discouraged unless there’s a really good reason for them to live there).
*   Single-item menus become buttons.
*   A “Translator” role can minimize the UI noise, similar to how we could show the state-change menu for people with the Reviewer role to ease their day-to-day dealings with the system.

In sum, these changes to the user interface make it less cluttered, more efficient, and easier to integrate with custom layouts, as the only thing you have to find space for in a custom design is a button and a pulldown.

Next, let's look at [better rich media handling in Plone].

[the introduction]: /simplifying-plone
[better rich media handling in Plone]: /plone-rich-media
