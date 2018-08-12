---
layout: post

redirect_from: "/articles/tab-matching-in-the-location-bar/"

title: "Tab matching in the Location Bar"
subtitle: "In Firefox 3.7, we want to bring tab matching to the location bar."
cover_image: mountains.jpg

excerpt: "In Firefox 3.7, we want to bring tab matching to the location bar."

---
One of the minor tweaks that we want in Firefox is the ability to switch tabs using the location bar. Yours truly has signed up to help shepherd this into the 3.7 release on the UI side.

This proposal is based on existing work from [Alex Faaborg] and [thoughts from Madhava Enros and Aza Raskin] around putting tabs in the location bar, and doesn’t stray very far from their proposals, so read those first.

There are some smaller changes we want to try, as they simplify the interface:

![](/images/switch-to-tab.png)

Apologies for butchering Faaborg’s original image for demonstration purposes. We might also want to tweak the wording to say something like “Switch to open tab” or similar to emphasize that aspect.

List the two choices adjacent to each other instead of using keyboard qualifiers like Shift or Alt

We don’t want to introduce modes here if we can avoid it.

Don’t show the URL as part of the entry for the already open tab

Loading a URL means loading the page — so we use this as an implicit indicator that the page will actually be loaded in the current tab if you select that entry.

The prioritized case is to switch to the active tab if one exists

We have to test different weightings vs. [frecency] and how it works in real-life scenarios, but we should be able to find a setting that makes it accurate and predictable.

Only the non-standard case needs a label

We don’t want to use a tab icon or similar here, since there’s already way too much visual noise in the AwesomeBar layout.1 1I’m hoping we can convince Messieurs Stephen Horlander or Sean Martell to give the location bar results some visual design love for Firefox 3.7. In particular, we should get rid of the underline in the current style, since underline generally signifies a link in the browser context, and is a poor choice for a highlighting mechanism. This will require some user testing — but if it’s too confusing, we could add in another label.

An upside of the combined approach is that the entries in the location bar results will keep the same size as the existing ones.

## Tab Matching Preferences

We will give people control of whether they want already opened tabs to show up in their location bar results. We add this setting to the location bar settings we already have in the preference pane:

When using the location bar, suggest:

*   History
*   Bookmarks
*   *Tabs*

Tabs will be off by default, as this is more of a “power user” setting. You have to explicitly choose to turn it on if you want it. This is done to minimize impact for existing users, and to keep the location bar behavior simpler.

We’ll do some user testing with it on by default and see how people react, but we suspect it will be confusing for a non-trivial number of our users. If it turns out it isn’t, we'll make it enabled by default.

## Related interface tweaks

We also need more data on how it should behave when you have multiple windows. The current thinking is that it will match any tab in any window. We’ll have to do some testing to see if this is too confusing for people, switching to a different window is definitely a different experience. We might end up restricting it to only match within the current window instead.

Another behavior we should fix as part of this improvement is how the Firefox UI behaves if you have the location bar hidden — currently you get the following dialog if you hit ⌘L to enter a URL when that part of the UI is hidden:

![](/images/open-web-location.png)

This is a remnant of the old “Open” dialog from earlier versions of Firefox. Instead of showing this, the location bar should slide down from the top and give you the interface you’re already familiar with, and disappear again once you have entered a URL. This way, you can hide both the location bar and the tabs, and just use the location bar to manage both. This also works well with full-screen mode.

We can always tweak this behavior once we have the initial implementation in place, but if there’s anything we didn’t think of — or improvements to the proposed approach — we’re always open to suggestions. In this case, please leave your comments on the [Mozilla Wiki page for Tab Matching] — thanks!

[Alex Faaborg]: https://twitter.com/faaborg
[thoughts from Madhava Enros and Aza Raskin]: http://www.azarask.in/blog/post/tabs-in-the-awesome-bar/
[frecency]: https://developer.mozilla.org/en/The_Places_frecency_algorithm
[“Power User” interface]: /tab-matching/#power-users
[Mozilla Wiki page for Tab Matching]: https://wiki.mozilla.org/Talk:Firefox/Projects/Tab_Matches_in_Awesomebar
