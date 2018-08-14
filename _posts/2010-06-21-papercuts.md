---
layout: post

redirect_from: "/articles/papercuts/"

title: "Paper Cuts in Firefox 4"
subtitle: "Update on paper cut bugs for Firefox 4"
cover_image: mountains.jpg

excerpt: "Update on paper cut bugs for Firefox 4"

---
A while back, I gave a presentation here at Mozilla based on feedback from a question thread posted to the Reddit web site: “[I work on the Firefox User Experience team, and this is your chance to tell me about your pet peeves].”

We got over 2 000 — yeah, that’s *two thousand* — replies, and spent some time analyzing and grouping the feedback into actionable bug reports and general focus areas, which was then presented to the Mozilla team and worldwide community in a talk here in Mountain View & broadcast on [Air Mozilla].

Unfortunately, the video recording was broken that day, but you can [view the presentation slides in HTML5 format here]. Copious amounts of presenter notes were added after the presentation was given, so you should be able to follow it even if you can’t see the recording.

The result of this presentation was that a number of bugs were filed under a “Paper Cuts” umbrella — in other words, the small, annoying issues that you encounter every day in any software — and an effort was started as part of the Firefox 4 effort to eliminate as many of these as we can during the beta period. They are similar in nature to what was called “polish” bugs in the Firefox 3 release cycle, but intentionally narrowly scoped, and managed aggressively to ensure that we have a smaller list that can be worked on.

## What’s so special about Paper Cut bugs?

I’m glad you asked! The paper cut bugs are issues that are of particular importance to the User Experience team, and as such, we will give priority to help you fix these bugs. It means that we’ll go the extra mile to try and support anyone that helps us with fixing them — so please [have a look at the list] and see if there’s anything you think you’re capable of fixing, and feel free to contact me directly or post questions in the bugs themselves if you need clarifications or help.

Some paper cut bugs are complicated and require some re-architecting — for instance, the modal dialog issues currently being handled by Justin Dolske & others — but a lot of them are small, self-contained, and the perfect thing to work on while you’re waiting for review for other projects.

## Our overall priorities

Of the 7 focus areas identified in the presentation, we chose to focus on two areas in particular for Firefox 4:

*   [Startup experience]
*   [Focus issues]

This doesn’t mean that the other bugs are any less important — but we obviously have to pick our battles to get stuff done. Don’t let this discourage you from handling bugs in the other focus areas, though!

## Our current Paper Cut Heroes

Several of the bugs have seen significant activity over the past month, and I’ll try to call out progress and general awesomeness over the coming weeks. Currently, excellent work is being done on:

*   [Eliminate modal dialogs] — Justin Dolske & others have been working on moving all dialogs to be tab-specific instead of window-specific. This means that HTTP authentication dialogs and JavaScript pop-ups no longer force you to answer them before you can switch to a different tab.
*   [Application updates in the background] — Rob Strong is currently working on a service that downloads & applies Firefox application updates transparently in the background, which means you don’t have to wait for the “Firefox is applying your updates” dialog when starting the browser anymore.
*   Add-on updates in the background — Dave Townsend, Jennifer Boriss and the rest of the team behind the new add-ons manager have made sure that updates to add-ons are less annoying, and don’t happen on startup anymore. You can still turn on explicit notification of updates if that’s how you roll, but it’s now automatic and transparent for most people.
*   [Improved tab behaviors] — Our intern extraordinaire Frank Yan has been submitting patches to eliminate spurious trackpad input (“fake bounce”) and several other improvements to tab handling.

…and lots of other issues, more updates to come in the next few weeks.

## Want to help out?

This week’s selection of paper cuts that we are currently looking for help in resolving:

*   [Bug #78414] — Application shortcut keys (keyboard commands such as F11, Ctrl+T, Ctrl+R) fail to operate when plug-in (Flash, Acrobat, QuickTime) has focus. This is mostly working on OS X, but Windows still has this problem. In general, any qualified keyboard shortcut shouldn’t be sent to the plugin, but go to the browser.
*   [Bug #566489] — Enable inline autocomplete again for URLs, but make it smarter. With the introduction of the Awesome Bar, we stopped “Speaking URL,” which affects our perceived performance, since it takes longer to enter a URL than it did before.
*   [Bug #492544] — Add “Paste & Go” + “Paste & Search” to context menu on location field + search field, respectively.
*   [Bug #565104] — Stop sites from opening popup or popunder windows, even in response to clicks. There’s a new type of “pop-under” that seems to be done by triggering a window open and then quickly focusing back on the current window. There’s no conceivable use for this in a web page/app, so this order of events should be detected, and disallowed.
*   [Bug #565760] — “Forget this password/login” on right click. It should be possible to remove a saved password from a form that is currently storing it.

If none of these issues tickled your fancy, take a look at the [full list of paper cut bugs] (tree view) — and pick something to fix!

***

## Overview of paper cuts

Here’s a quick overview of the focus areas we are trying to fix, along with links to the bugs that collect these in focus areas:

### Focus issues

[Tracked in bug 565510], contains items such as:

*   Web sites should not be able to steal focus when in chrome
*   Flash, PDF and other plugins steal focus shortcuts, can’t open new tab using keyboard when inside a YouTube video or a PDF, back button stops working
*   Avoid app-modal dialogs, make them tab-modal

### Startup Experience*

*Not what you think, focus on perceived performance instead of milliseconds for startup time.

[Tracked in bug 565511], contains items such as:

*   Rebuild profile on upgrade/reinstall
*   “Firefox is open but not responding” alert
*   Don’t ask about updates all the time
*   Ideally: upgrade in the background — if not, upgrade at shutdown, not startup!
*   Shutdown takes a long time for some users (30-40 seconds)
*   Include BarTab-like behavior by default
*   Put “Restart” and “Restart & Save Session” in “Exit” combomenu in the new design?

### Being in Control

[Tracked in bug 565512], contains items such as:

*   Control over audio in tabs!
*   Web sites shouldn’t be allowed to resize windows they didn’t create (i.e. main window)
*   Pop-under ads are back, worse than ever
*   Put the user in control of generated windows
*   Opening new windows should be user-controllable
*   SSL: Add an “I know what I’m doing” option to self-signed certs
*   Include option to delete Flash cookies
*   Fix Master Password
*   Don’t let a web app be able to lock up a tab
*   Browse-by-name is inconsistent and confusing
*   Make a hotkey to list shortcuts + show them in the right-click menus
*   Enable autocomplete again, but make it smarter
*   Better preview, especially for PDF & types we already have viewers for

### Add-ons & Plug-ins

[Tracked in bug 565513], contains items such as:

*   Don’t wait 3 seconds when installing add-ons from AMO or the inline add-ons browser
*   When an add-on is outdated / no longer maintained or working, suggest alternatives
*   Click-to-activate for plugins — default for rare-but-perf-killing things like Java
*   Don’t let add-ons open new pages
*   Don’t let apps install add-ons without consent
*   Fix the crufty status bar, extensions add clutter

### Tab Behaviors

[Tracked in bug 565515], contains items such as:

*   Opening tabs at the end when new & next to when context-triggered is confusing
*   New tabs should keep history from the tab that spawned them
*   Tab behavior is very personal, expose more prefs here
*   Make it easy to close lots of tabs
*   Fix tab stack ordering
*   Fix Ctrl-tab to use stacks, so it can be used to compare between 2-3 tabs
*   Tab reloads when it is detached; this is annoying and unexpected/dangerous
*   Don’t separate windows and tabs in the undo stack
*   Let me fit more tabs in the bar, make it easier to find the ones I can’t see
*   Blank tabs when clicking should never appear
*   Can we fix the "Cmd-click window void"?
*   Let me have Private and normal windows at the same time
*   Ask when hitting ⌘Q
*   Better load indicators — separate activity from progress
*   Detect whether your webmail client is already open, don’t open new window
*   Eliminate fake “bounce” when scrolling tab bar, you can’t see when you’re at the end

### UI Cruft & Consistency

[Tracked in bug 565517], contains items such as:

*   Make search field tab-specific & clear when navigating away from results page
*   Find in page should to be page-specific, and close when you click outside it, move to top
*   Change “Save” to “Download”
*   Work offline must die
*   Fix downloads!
*   Fix mess + ordering in contextual menus; common options first
*   “Search Google for…” context menu entry should be in textareas & inputs too
*   Fix selection behaviors
*   “Paste & Go”; “Paste & Search”
*   A useful full-screen browsing experience
*   Make Personas work better with the default theme, make them easy to remove
*   Favicons on OS X bookmarks toolbar + multiple rows supported

### OS Integration

[Tracked in bug 565518], contains items such as:

*   Multiple screens behavior on Windows is wonky, can we fix?
*   Additional Win7 features: Jump lists, windows in Aero Peek, download progress
*   Enable NTLM authentication for intranets
*   MSI installer with group policies
*   Hard to assign default apps on Linux
*   Use of Keychain on OS X

[I work on the Firefox User Experience team, and this is your chance to tell me about your pet peeves]: http://www.reddit.com/r/AskReddit/comments/boipt/i_work_on_the_firefox_user_experience_team_and/
[Air Mozilla]: http://air.mozilla.com/
[view the presentation slides in HTML5 format here]: http://www.scribd.com/doc/31643187
[have a look at the list]: https://bugzilla.mozilla.org/showdependencytree.cgi?id=561125&maxdepth=2
[Startup experience]: https://bugzilla.mozilla.org/showdependencytree.cgi?id=565511&maxdepth=2
[Focus issues]: https://bugzilla.mozilla.org/showdependencytree.cgi?id=565510&maxdepth=2
[Eliminate modal dialogs]: https://bugzilla.mozilla.org/show_bug.cgi?id=59314
[Application updates in the background]: https://bugzilla.mozilla.org/show_bug.cgi?id=489138
[Improved tab behaviors]: https://bugzilla.mozilla.org/show_bug.cgi?id=565515
[Bug #78414]: https://bugzilla.mozilla.org/show_bug.cgi?id=78414
[Bug #566489]: https://bugzilla.mozilla.org/show_bug.cgi?id=566489
[Bug #492544]: https://bugzilla.mozilla.org/show_bug.cgi?id=492544
[Bug #565104]: https://bugzilla.mozilla.org/show_bug.cgi?id=565104
[Bug #565760]: https://bugzilla.mozilla.org/show_bug.cgi?id=565760
[full list of paper cut bugs]: https://bugzilla.mozilla.org/showdependencytree.cgi?id=561125&maxdepth=2
[Tracked in bug 565510]: https://bugzilla.mozilla.org/showdependencytree.cgi?id=565510&maxdepth=2
[Tracked in bug 565511]: https://bugzilla.mozilla.org/showdependencytree.cgi?id=565511&maxdepth=2
[Tracked in bug 565512]: https://bugzilla.mozilla.org/showdependencytree.cgi?id=565512&maxdepth=2
[Tracked in bug 565513]: https://bugzilla.mozilla.org/showdependencytree.cgi?id=565513&maxdepth=2
[Tracked in bug 565515]: https://bugzilla.mozilla.org/showdependencytree.cgi?id=565515&maxdepth=2
[Tracked in bug 565517]: https://bugzilla.mozilla.org/showdependencytree.cgi?id=565517&maxdepth=2
[Tracked in bug 565518]: https://bugzilla.mozilla.org/showdependencytree.cgi?id=565518&maxdepth=2

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
