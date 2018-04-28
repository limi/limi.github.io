---
layout: post

redirect_from: "/articles/firefox-mac-installation-experience-revisited/"

title: "Firefox Mac installation experience, revisited"
subtitle: "A second look at the issues with installation of firefox on the mac, with recommendations on how to fix them."
cover_image: mountains.jpg

excerpt: "A second look at the issues with installation of firefox on the mac, with recommendations on how to fix them."

---

<span>Recently, I posted an article</span> on how to [improve the Firefox installer experience] on the OS X platform, and how we were looking at making the user experience better for first-time Mac users and people coming from other platforms.

After posting the first article, we continued investigating how to make the installation experience better, and a developer urged us to check out how [Delicious Library] did it — by giving people the option to move the application to the right folder when it was first launched.

A few days later, the article got picked up by a number of Mac blogs and news sites — among the most well-known: [Daring Fireball], [Digg], [TUAW], [OSNews], as well as the French [MacGeneration].

Soon, the hate mail started trickling in. “Nothing is wrong with the Mac install experience.” “People that can’t teach themselves how disk images work shouldn’t be allowed to use a Mac!” *&*cetera. I wish I was making this up, but these *are* from real emails. Fun stuff.

But there was also a surprisingly large, second group of people: software developers and support people. To quote from one of the emails:

> “As a former Mac Genius I have literally seen hundreds (thousands?) of instances of the issue you describe. I have heard the confusion regarding two Firefox icons, and ‘why does it always take so long?’ \[when starting an application\]”

They all offered up different ways to get around the issues with disk image-based installs, but there was one common thread in all their responses:

*The disk image-based process for installing applications in OS X is broken.*

We mentioned the Delicious Library install behavior earlier — Daring Fireball linked to [great write-up] from the developer of the application [The Hit List] — where developer Andy Kim describes this move-on-launch behavior in detail. Even cooler, he released [the code to do this] as open source.

In short, it detects whether the application gets launched from a disk image or the downloads folder, and offers to move the application to an appropriate location:

![](/media/applications-move.png)

As a long-time Mac user, I strongly agree that installers shouldn’t be used unless absolutely necessary. But there’s also a difference when you have an audience as big as Firefox does, and different concerns that were minor with a smaller user base suddenly become a significant roadblock for adoption. This is why we initially investigated having an installer.

However, given the option to detect when people are launching it from the disk image or the Downloads folder, we can accomplish what we want without an installer. When we saw the Delicious Library[^1] behavior, the installer option was no longer something we wanted to pursue.

## Our options, given an installer-less setup

Ship a DMG like we currently do, and add disk image detection.

Pro: Provides a dedicated disk image window in the Finder with only one choice — presentation is nicer, and it’s more obvious what to do next. Makes it easy to see which version of Firefox the disk image contains.

Con: Leaves the disk image behind, unless we include code to unmount *&* delete the disk image as part of the application.

Ship as an internet-enabled DMG or as a ZIP file, with the “Downloads” folder detection.

Pro: Both ZIP files and internet-enabled disk images automatically unpack, leaving less clutter on the file system.

Con: Leaves the file in the “Downloads” directory, where it may not be visible if there’s a lot of clutter. It also makes it harder to see which version of Firefox you have downloaded.

We’re partial to the internet-enabled DMG solution since it’s less involved, leaves fewer stray files on the system — it unpacks the the disk image <abbr title="and">&</abbr> removes it, leaving you with just the application-but allows you to retrieve it from the Trash if you really need it. and requires less of a change to the current installer build process.

Our — admittedly minor — issues with ZIP when compared to the DMG approach are:

*   Unpacking the ZIP file will leave the original archive in the “Downloads” folder, so you have to remove it manually — unless you changed that setting. We want to leave as little clutter as possible.
*   The CRC error checking that ZIP uses to verify its files is slightly less robust than the DMG equivalent. It’s usually good enough, so this is also a minor issue.

Considering this, and with the goal of making the change as non-intrusive as possible for our current build setup, the internet-enabled disk image is our preferred approach.

## The new Firefox installation experience

After considering all of the above factors, we believe the Firefox installation experience on OS X should ideally look like this:

1.  Initiate the Firefox download.
2.  When download completes, Safari will unpack the disk image, throw away the DMG file, and show a Firefox icon in its download window — as well as selecting it in the Finder in the background.
3.  When you double-click the Firefox file, it gives you the options to:
    *   Move Firefox to the Applications folder
    *   Add Firefox to the Dock
    *   <abbr title="and">&</abbr> set Firefox as your default browser.…all of these actions are optional, of course.

We believe this gives us the best of both worlds — predictability and flexibility for the power users, and simplicity and for people that are new to the Mac. We hope you agree.

As explained in the previous article, the Firefox team is currently busy getting the 3.6 release out on time — so if you’re a Mac developer with some spare cycles to make this happen, [get in touch]. You even have [the code to get you started].

[^1]: I don’t actually know who invented this behavior, but it’s a very elegant solution given the constraints.

[improve the Firefox installer experience]: /mac-installer
[Delicious Library]: http://delicious-monster.com/
[Daring Fireball]: http://daringfireball.net/2009/09/how_should_mac_apps_be_distributed
[Digg]: http://digg.com/apple/The_confusing_art_of_installing_apps
[TUAW]: http://www.tuaw.com/2009/09/21/the-confusing-art-of-installing-apps/
[OSNews]: http://www.osnews.com/story/22195/Improving_the_Mac_OS_X_Application_Installation_Process
[MacGeneration]: http://www.macgeneration.com/news/voir/136495/firefox-nouvel-installeur-et-abandon-de-tiger
[great write-up]: http://www.potionfactory.com/node/251
[The Hit List]: http://www.potionfactory.com/thehitlist/
[the code to do this]: http://github.com/potionfactory/LetsMove/
[get in touch]: http://groups.google.com/group/mozilla.dev.apps.firefox/topics
[the code to get you started]: http://github.com/potionfactory/LetsMove/

*[AMO]: addons.mozilla.org
*[CDNs]: Content Delivery Networks
*[CPUs]: Central Processing Units
*[CPU]: Central Processing Unit
*[CSS]: Cascading Style Sheets
*[DMG]: Disk Image
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
