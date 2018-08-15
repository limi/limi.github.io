---
layout: post

redirect_from: "/articles/improving-the-mac-installer-for-firefox/"

title: "Improving the Mac installer for Firefox"
subtitle: "With a renewed focus on improving the Firefox install experience — what can we improve on the Mac?"
cover_image: mountains.jpg

excerpt: "With a renewed focus on improving the Firefox install experience — what can we improve on the Mac?"

---

Lately, the [Metrics team] here at Mozilla have conducted some great research into why some people leave the installer before Firefox is finished installing, [uncovering a couple of bugs in the process], and improving the way we ask people [what will be their default browser].

(Also make sure you read [part 2 of this article])

The team looked specifically at the Windows installer — which makes sense, since that’s where most of our new users are coming from — but there are some substantial issues with the Mac install experience that we will explain in more detail.

## Where Apple failed

There’s a lot to like about Mac OS X, but in their quest to make things as simple and uncomplicated as possible, Apple actually made some things worse for the (mythical) average user, as well as for people new to the Mac. One of these areas is application installation.

If you are unfamiliar with the Mac OS X install process, let us quickly outline how application installation on Mac OS X works:

1.  Download the file.
2.  Open the disk image.
3.  Drag the application to your Applications folder.
4.  Optionally add the application to the Dock.

While this process is familiar to experienced Mac users, and removes the need for uninstall scripts, very few new users that recently purchased their first Mac know how to do this. Unless they have a friend that has shown them how it works, it doesn’t make sense to anyone coming from a different platform.

To mitigate this, the download page for Firefox — as well as pretty much every other Mac application on the planet — attempts to tell you what to do once you have downloaded the disk image:

![](/media/firefox-three-steps.png)

The problem with this should be obvious — by the time the download is complete, that web page is usually long gone, and many people don’t read these kind of instructions at all.

To make matters even worse, Firefox is often one of the first applications that people new to the Mac go to download and install, since they want the familiar browser they have used earlier. They might catch on to how this works later, but it’s a fair bet that they don’t know this as new Mac users.

Some common errors that we have seen repeatedly among informal testing with friends and family are:

Dragging the application to the dock directly

: This creates a link to the file *inside* the disk image, which means that every time they try starting Firefox, the disk image is unpacked and mounted, and starting of Firefox becomes very slow, which makes it a bad experience.

Thinking that starting Firefox is done by opening the disk image every time

: This is very common, and the logic is that the first time they started Firefox, they had to do this, so they continue doing it. This makes starting Firefox a chore, since it takes a lot of clicks to accomplish.

What’s interesting is that Apple *are* aware of this problem, and even added a warning that shows up when you try to run an application from the disk image:

![](/media/firefox-image-warning.png)

Quick, spot the problem! It doesn’t actually tell you anything about *why* it’s a bad idea to run stuff from a disk image — or even what you can do to avoid the problem — they just state that you’re about to do so. Oh, and that the file is from the Dangerous Internet. For a company that [prides itself on informative dialog boxes], this is quite surprising.

## How we can make the Mac install experience better

We can make some simple fixes to the current approach to make the experience better for people new to the Mac, while still retaining the power to put the file anywhere, per the classic OS X model.

What problems do we want to solve?

: People that don’t understand the standard installation process on Mac should be helped towards their goal.
: Downloading Firefox and then forgetting about the download should be less common.
: Maintain the standard installation model for the users that prefer the drag & drop install method.
: Make it possible to set Firefox as your default browser during the install process.

How do we fix these problems? By supplying a dedicated installer in the disk image, similar to the standard Mac OS X application installer:

![](/media/plone-installer.png)

We can fix all of these by using some capabilities of the standard Mac package installer and disk image creation tools supplied by Apple. In particular, they have features where you can [run an installer when a disk image is mounted], like Apple does with some of their own disk images. This, combined with the auto-mounting of disk images downloaded via Safari, gives us an installation flow that is likely to not lose people along the way.

The final installation flow should look like this:

1.  Start the Firefox download.
2.  When the download is complete, the disk image will mount automatically (if they were using Safari), and the Firefox installer runs.
3.  The install procedure continues similar to how it happens on Windows.
4.  As the last step of the process, the installer lets you set Firefox as the default browser, and start the application immediately. We have seen users forget that they just installed Firefox if you don’t let them start it at the end of the process, and make that the default choice.

Note that experienced Mac users should be able to cancel the installer at any time and drag the Firefox application to the location they want instead, thus there should be no loss of functionality or flexibility for them.

## Help us make it happen

At Mozilla, we’re currently in crunch mode to get Firefox 3.6 ready, and therefore we have limited resources to look at creating a proper installer for Mac OS X right now. If you’re an experienced Mac OS X developer that would like to help out with creating a new installer that we can help test, we’d love to hear from you. Please send an email to the [dev.apps.firefox Google Group] — or via the [traditional mailing list] interface. If you want to track progress, we have filed a [bug for the installer improvement] that you can subscribe to.

Also make sure you read [part 2 of this article].

[part 2 of this article]: /mac-installer-revisited
[Metrics team]: http://blog.mozilla.com/metrics/ "Blog of Metrics"
[uncovering a couple of bugs in the process]: http://blog.mozilla.com/metrics/2009/07/30/an-improved-experience-for-2000000-non-firefox-users/ "An Improved Experience for 2,000,000 non-Firefox Users"
[what will be their default browser]: http://blog.mozilla.com/metrics/2009/08/03/more-changes-coming-to-the-firefox-installer/ "More Changes Coming to the Firefox Installer"
[prides itself on informative dialog boxes]: http://developer.apple.com/mac/library/documentation/UserExperience/Conceptual/AppleHIGuidelines/XHIGWindows/XHIGWindows.html#//apple_ref/doc/uid/20000961-TP10 "Apple Human Interface Guidelines: Alerts"
[run an installer when a disk image is mounted]: http://osdir.com/ml/os.opendarwin.webkit.devel/2007-12/msg00037.html "Auto-launch installer after download"
[dev.apps.firefox Google Group]: http://groups.google.com/group/mozilla.dev.apps.firefox/topics
[traditional mailing list]: https://lists.mozilla.org/listinfo/dev-apps-firefox
[bug for the installer improvement]: https://bugzilla.mozilla.org/show_bug.cgi?id=516362
[a part 2 available]: /mac-installer-revisited

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
