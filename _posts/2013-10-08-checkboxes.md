---
layout: post

redirect_from:
  - "/checkboxes-that-kill/"
  - "/checkboxes-kill"

title: "Checkboxes that kill your product"
subtitle: "A little historical baggage can be a dangerous thing when multiplied by a few hundred million individuals"
cover_image: mountains.jpg

excerpt: "A little historical baggage can be a dangerous thing when multiplied by a few hundred million individuals"

author:
  name: Alex Limi
  twitter: limi
  bio: VP Product at Highfive
  image: ks.png
---

If I told you that a company is shipping a product to hundreds of millions of users right now, and included in the product are several prominent buttons that will break the product completely if you click them, and possibly lock you out from the Internet — can you guess which product it is?

Sounds like that’s the kind of product that only a large enterprise software company like Oracle or IBM would ship, right? Maybe some of the antivirus extortionware software for Windows? Maybe VPN software?

Well, [we have met the enemy, and he is us].
In the currently shipping version, Firefox ships with many options that will render the browser unusable to most people, right in the main settings UI.

***

How did we get to this point with Firefox? Most of these options exist for historical reasons — whenever there’s a new feature, it often gets a checkbox to turn it off. The other common case is when a feature isn’t obviously useful to everyone, and it’s hard to make an obvious choice about whether to have it enabled by default or not — so we build in a switch. Or sometimes the person implementing it thinks it should have a switch, and nobody stops to ask if it’s a good idea.

I’m not going to retread discussions about this, there are many versions of that article across the web — the main point is that it is usually a failure of design, and a failure to agree on sensible default behaviors. Options are “the cheap way out,” and they usually speak to an inability to agree on what to do in a given situation.

**Design by committee often looks like a row of checkboxes.**

What I *do* want to put the focus on, however, is that you *have* to perform an audit of your product every so often and see how the people using your product have changed, and what kind of functionality that made sense at the time may not make much sense anymore.

Of course, we should start in our own backyard, so here are some obvious examples from Firefox. These are things we need to fix:


## Load images automatically

From the Content panel in our settings, try unchecking the box:
![Content Panel](https://i.imgur.com/Xxo6hmw.png)

Here’s how Google’s front page looks like if you uncheck that box:

![Google](https://i.imgur.com/3aLJmS2.png)

That’s right, you can’t even see the text box you’re supposed to type your search into. Congratulations, we just broke the Internet.

Of course, there are legitimate uses for this, from low-bandwidth situations to web development testing, but that’s where our [excellent add-ons ecosystem] comes to the rescue. Are more than 2% of our users likely to use this setting on a regular basis? Probably not.


## Enable JavaScript

More fun from our Content panel, you can turn off JavaScript with a simple click:

![Turn off JavaScript](https://i.imgur.com/Xxo6hmw.png)

Try booking a travel on Hipmunk without JavaScript:

![](https://i.imgur.com/c6OOjp7.png)

Most sites these days that aren’t just displaying content will fail in interesting & mysterious ways if you don’t have JavaScript enabled. For the general population, Firefox will appear broken.

> Fun historical fact: If you disabled JavaScript in Netscape 4, CSS would also stop working — because CSS was applied to the page using… JavaScript.

And yes, I know that some people have reasons (privacy, web development) to turn off JavaScript. There are many add-ons that can help with this — but it’s not something that we should ship to hundreds of millions of users.


## Turning off navigation

Firefox is very customizable! In fact, it’s so customizable that we allow you to make the browser unusable with a single click. Try turning off the Navigation Toolbar, for instance:

![Navigation Toolbar in Menu](https://i.imgur.com/W2QOeWQ.png)

Good luck trying to find a web site that can help you fix this problem when your son was clicking around the menus in Firefox yesterday, and today your browser has no buttons:

![Browser with no buttons](https://i.imgur.com/0JmJ60q.png)


## Turning off SSL & TLS

Now we’re getting to the “shooting fish in a barrel” category — there are many fun options in this preference panel:

![Preference Panel](https://i.imgur.com/Q1SRzmH.png)

If you turn off SSL or TLS, Gmail, Google Reader, and other Google services will look like this:

![Error Message](https://i.imgur.com/FjDK3dn.png)

Note that we don’t even tell you that you can turn it back on in the settings. We just tell you that it has been disabled, and that you should “contact the website owners to inform them of this problem.”

Good luck trying to do that when you can’t even see the web site or load your email.


## The entire certificate manager

Oh boy, where do we start?

![Certificate Manager](https://i.imgur.com/YxVQID7.png)

Personally, I feel pretty confident that the number of people that know how any of this works can be narrowed down to the people that work in these companies in the list. At least not too far off.

This entire thing — possibly with the exception of the personal certificate list — needs to be moved out into an add-on for people with interest in managing their own certificates. It’s our job as a browser to keep you safe, we shouldn’t outsource this to individuals.

You probably don’t want your bank to look like this because two days ago, you read an article on the Internet — authored by who-knows — telling you to remove an entry in your certificates “for added safety”:

![Bank Site Error](https://i.imgur.com/Zn3auty.png)

Also, is that an NSS Internal PKCS #11 module in your pocket, or are you just happy to see me? Do I need to enable my FIPS?

![NSS](https://i.imgur.com/eLDED0a.png)

As Privacy Engineer Monica Chew at Mozilla asks, “[Is it really worth having a preference panel that benefits fewer than 2% of users overall?]” — obvious spoiler alert: The answer is no.

On the flipside, from that same article: how many users have we broken the web for when 1.6% of them may have TLS support turned off, and possibly not be aware of it? Even 1% of a few hundred million isn’t a trivial number of people.

The people that need to do these things should use add-ons, or at the very least an <code>about:config</code> tweak.


## Override automatic cache management

Another way to slow down the browser and make Firefox look bad is to give it no disk space to use for caching:

![Cache Management](https://i.imgur.com/91N6zzP.png)

What about computers with very little disk space? Shouldn’t you be able to restrict how much disk space is being used? It turns out, we know that you are low on disk space, and will reduce our usage accordingly. It’s pretty likely that Firefox keeps better track of this than humans do. So let’s make computers do what they’re good at: keeping track of numbers.

***

What have we learned? There are a lot of options in our products that are used by very few people, and some of them can have *disastrous* effects. We’re trying to design software that can be used by everyone — that also means we have to keep them safe and not make it so easy to break a product they rely on every day. None of these are put there with malicious intent — some of them  even made sense at the time — but it’s time for us to do some scrubbing and cleaning of the Firefox settings.

What about the product that *you* are building? Is it time to take a fresh look at what kind of options you include?

<small>Thanks to Frank Yan, Blake Winton, Tony Santos, Monica Chew, Sid Stamm & Madhava Enros for reading drafts of this.</small>

[excellent add-ons ecosystem]: https://addons.mozilla.org
[Is it really worth having a preference panel that benefits fewer than 2% of users overall?]: https://monica-at-mozilla.blogspot.com/2013/02/writing-for-98.html
[we have met the enemy, and he is us]: https://en.wikipedia.org/wiki/Pogo_%28comic_strip%29#.22We_have_met_the_enemy_and_he_is_us..22 "Quote from Pogo, the comic strip"

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
*[NSS]: Who knows?
*[PKCS]: Really?
*[FIPS]: Seriously.
