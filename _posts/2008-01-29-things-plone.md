---
layout: post

redirect_from:
  - "/articles/18-things-i-wish-were-true-about-plone"
  - "/articles/18-things-i-wish-were-true-about-plone/index.html"

title: "18 Things I Wish Were True About Plone"
subtitle: "What should Plone improve?"
cover_image: mountains.jpg

excerpt: "What should Plone improve?"

---

Let me preface this with the obvious disclaimers for the readers that aren’t intimately familiar with the Plone community and culture:

*   This is my personal list, and doesn’t necessarily reflect what the Plone Community wants to do.
*   Even though I work at Google, there’s nothing here that constitutes knowledge of any secrets about future products. Any mention of Google here is as an outsider looking in, as I don’t work in the below mentioned focus areas — and have no insights into what they are up to next.
*   Some of these are confrontational, some are not — they are all here to stimulate discussion, and not to imply failure or dissatisfaction in any of the areas. The Plone community is a fantastic community *exactly because* we can have these sorts of discussions out in the open without people taking it personally.
*   Make no mistake — we are also a leader in a lot of the areas mentioned — I’m just making sure we keep pushing the curve and continue delivering the best damn content management system on the planet.

This list is merely a brain dump meant to inspire and provoke discussion in the days leading up to the [Plone Strategic Planning Summit] at the Googleplex in February 2008. The discussions there will certainly be a bit more high-level than what I talk about below, but this is a good chance for you to read about some of the baggage I will bring to the summit — as well as add your own views to the discussion so we can make note of them.

With that out of the way, get a cup of your favorite beverage, and let’s move on to the list! These are in no particular order, but I numbered them to make it easier to keep track of how far along you are in the document.

***

## #1: Simple development should be possible entirely through the web interface

For those of you familiar with Zope, this point can be summarized as “Deliver on the promise of ZClasses”. Yes, I said ZClasses. For those of you unfamiliar with Zope, ZClasses is an old — and now frowned-upon — part of Zope that allows you to construct content types entirely through a web interface. It is what I usually refer to as the “gateway drug” of Zope. Most people that got into Zope in the beginning did so especially based on what ZClasses were supposed to deliver.

Of course, history shows that this didn’t really work out, as there was no transition path from clicking around and making your own simple type definitions, and then move to more advanced programming later on. With Zope 3 and the tools we now have at our disposal, it’s time to revisit this notion, since we can pull it off without any serious drawbacks this time around.

The path to get started with Plone should be:

1.  Install Plone, change options and settings to your liking, add simple types for the main business processes you want to mimic — all using the web interface.
2.  When you have the need for more advanced types — or just want to make sure the setup is repeatable and possible to check in to a code repository or a product: click a button → get a filesystem-based product.
3.  Once you are comfortable with the architecture and general approach, you can continue doing the rest using a filesystem-based approach to development.

I know [Martin Aspeli] has done a lot of thinking around this, and has a proposal based on Zope 3 technologies ready, so I think this is already in good hands — but worth exploring and giving feedback on. My major goals to ease the approachability of Plone is to be able to solve the following common tasks using this solution:

1.  Create simple types — then serialize (and diff) to an FS product
2.  Change setup and configuration — then serialize (and diff) to an FS product
3.  Create user properties — then serialize (and diff) to an FS product

Zope is one of the few systems that can do this, and we should exploit it for what it’s worth. The ultimate step is always filesystem-based development, but the goal is to be able to do all the simple things without programming — and then be able to dump to file system when you are ready to take it to the next level.

## #2: Themes should not require Plone-specific knowledge

There are several reasons why there aren’t a lot of high-quality themes available for Plone. Most of it is a reflection of the fact that Plone is a more sophisticated solution that tends to have paying customers and businesses as a core audience. Obviously, this isn’t conducive to a large theme library in the same way as blogging software like WordPress — simply because companies and organizations pay a lot of money for their designs, and don’t want other web sites looking like their site.

The other reason is that it currently requires a lot of CSS knowledge to work with Plone to produce a theme that will be maintainable as well as being future-proof. Separating the theme layer from the main templates in a way that makes it possible to hand a template to a web designer independently of Plone is what we have always wanted to do.

This reality is getting closer with the arrival of Deliverance, an approach that makes a separate “theme layer” that is independent of Plone, and can be worked on without having any knowledge about how Plone works. Deliverance is currently being used by several sites, and is maturing rapidly due to the great work of [Paul Everitt] and the [OpenPlans Team]. I believe having Deliverance — or something similar — as part of the Plone core would be a sensible and game-changing move, provided it can deliver on its promises.

Again, this is something that I believe is in good hands, but needs shepherding, cheer-leading and feedback to make sure we make it as simple as possible to put your own look on Plone.

## #3: Fantastic searching, grouping & batching operations for content should be accessible to everyone

The best way to sum this up for those of you who are familiar with Plone already would be *Collections on steroids*. There is a real need to organize and group content in Plone in various ways, and the current Collection and search implementations in Plone barely scrapes the surface of what we’re capable of in this area.

*   The first step would be to mimic a slice/dice interface that all Plone developers are intimately familiar with: the Trac search. For those of you unfamiliar with Trac, it looks a lot like the Smart Folders in Mac OS X. The goal would be to offer similar query — and especially grouping — abilities in the core Collection type and also the advanced search form. This alone would make Plone a fantastic tool for content management.
*   Assigning metadata to items in a collection of content — be it permanent or temporary — should be possible for all content. Construct your collection of content using the search/grouping interface and then apply tags, expiry dates and contributor metadata to it is a necessity. I cannot stress this enough.
*   Another related quick fix would be to treat CSV as a first-order citizen for export/import of data — especially in Collections and users/groups — as well as wherever else data comes in a tabular format. CSV is the lingua franca of structured data and data manipulation/exchange, and we’re not exposing this in Plone at all right now. Since Python has great libraries for handling CSV data, it’s time we start using them.
*   In the same vein, faceted browsing of the search results to make it easy to narrow down the result set. This is essentially the same back-end with a slightly different UI on top.

## #4: Rich media should be a seamless part of the Plone Experience

One of the major growth areas we see in the Ploniverse is the need to handle rich media in a graceful and elegant way. Audio and video data in particular, but also interactive media like Flash.

This presents a number of challenges on both the front-end and back-end:

*   **On the back-end:** Seamless and efficient storage of large files (aka. “BLOBs”), and performance in serving up these without using too much memory or CPU on the server side
*   **On the front-end:** Easy upload of a large number of files, as well as playback and dedicated view modes for audio, video and picture collections.

The back-end part of this equation has received a lot of attention lately, with Zope 2.11 shipping with native BLOB support that massively improves the memory and storage requirements as well as the performance of serving up large files. On the Plone side, [Andreas Zeidler] has done an amazing job and created an implementation that makes this available on the Plone side of things, and this work is scheduled to land in Plone 4.0.

On the front-end, projects like [Plone4Artists] have explored a number of different approaches to handling these types of data in Plone, and there are several important lessons to learn there.

## #5: Streamlined, unified install experience on all platforms

Plone already has the best install experience of any content management system — it’s really the Gold Standard in how easy you can make it to get started with such a sophisticated framework across all the major platforms. So why do we need to change it? The reason is simple: recent progress towards integration and unification with the Python world — especially eggs (similar to JAR files for those of you from the Java world) — necessitates some improvements here.

The current situaton is also that we have three separate cores of the installers, one for each of Windows, Mac OS X and Unix/Linux. The more we can share between these implementations, the easier it becomes to release software as well as troubleshoot problems regardless of what platform people are on.

Steve McMahon and Kamal Gill have done great work done on updating the Unified Installer to be egg-based recently, which lays most of the groundwork in getting the Windows and Mac OS X installers to use the same infrastructure. I want to make sure Windows is not left behind, as this is a very common *evaluation* platform for Plone — even if people end up installing it on Linux/Unix-based platforms when it goes into production.

However, I want to take this further, and think about the next steps in both initial install experience, upgrade experience, and how to install add-on products. Going into great detail on this would necessitate an entire document in itself, but I’ll list my current brainstorming notes in this area. If you have any feedback on this, please help out, these are loosely joined ideas that may or may not make sense — or even be realistic:

1.  Install Plone/Zope in a consistent way that is 100% insulated from other changes to the system Python and other Plone/Zope instances on the system.
2.  Install add-on products without having to do the download/unpack/locate directory dance. One way to do this would be to let “known good” projects be installed using the web interface, others might require server access. “Yes, I have a backup” button when installing.
3.  Ability to ask questions during *install* — before the add-on product gets installed. In a lot of cases, this will influence how the add-on is installed and set up.
4.  Ability to ask questions during *upgrades*: “We changed the default policy for X to do Y, but unless you want us to change your default setting, we will leave it the way it was.”
5.  Show an indicator on whether we consider it safe to upgrade to the next available version X of Plone, based on the dependency information listed in the currently installed eggs — ie. if all add-on products say they work with the next version of Plone, tell the user this. If not, let him know which ones don’t have support yet.
6.  Is it possible tocreate a roll-back mechanism to downgrade installs in case add-ons screw something up? Yes, you should always have backups, but it would still be a great addition that would alleviate a lot of nervousness on behalf of the site admin.
7.  An easy way to clone your production instance to a development server.
8.  Upgrade Plone/Zope using the web interface with just selecting to do so — ask for backup locations, etc.
9.  Make the installer more like a "network installer" — ie. comes with the core, and then gets the relevant eggs online. Could even check for newer versions when you run it, and ask if you want the newer version instead. Minimizes the initial download, and makes it easier to handle release files. The role of the installer: set up a sane, insulated environment with Python + easy_install that works!
10.  [Andy McKay] had a great idea of super-simple, standalone ZPT-based (not Zope!) application to replace the Plone Controller — making it cross-platform. Upgrade options, test running and so on could also be added here.

## #6: Increased focus on a culture of systematic benchmarking and performance tuning

Like in Alcoholics Anonymous, the first step to healing is to realize that you have a problem. Unoptimized Plone sites tend to be quite slow, and while there are numerous ways to make them go faster, they all require additional knowledge from the integrator. We can do a lot with a little investment here, as earlier optimization efforts have shown. We need to get rid of the attitude that “there’s not much we can do about performance, Plone is a bit slow since it does so much for you”. Every single time we have examined this closer, there are huge wins to be had on a number of levels.

The complimentary side of this is to establish a nightly testing framework, in the same manner we already run automated, comprehensive code tests every night. Knowing *when* something started being slower is just as important as speeding up things — especially since we rely on the combined stacks of CMF, Zope and Python. A small screw-up can have massive consequences for us, and it can be hard to track down what changed. If you have a day-to-day log of some central performance statistics, you can narrow this down to specific changes — both upstream and in Plone itself.

Some of the things we can do:

1.  Nightly speed benchmarks (we want to know *when* code was added that made things slower)
2.  More analysis on where the CPU time goes — will need Zope gurus here, as it’s very opaque to analyze at the moment, we tried this with [Mike Solomon] (Python performance guru at YouTube), and it’s really hard to know what the components are for an outsider — even for our seasoned, battle-scarred Plone veterans.
3.  Graph dependencies in an attempt to clean up code and make things simpler. The current inheritance trees in Plone + CMF + Zope are quite convoluted at times, and there’s probably a lot of simplification potential here once we see the bigger picture.
4.  I want a random content population mechanism! To do proper automated benchmarks, you need a realistic body of content to test it with. Luckily, [Tarek Ziade] and others at the Snow Sprint 2008 [already started work on this].

## #7: Improved tabular data story

Right now, Plone is a great choice for content-centric applications, and it also has the possibility to integrate with more structured, relational data via the SQL database adapters. However, most of these solutions are old — and while they work, Python has acquired a number of great SQL integration tools in the meantime, SQLAlchemy being the weapon of choice for most relational-thinking Pythonistas.

I believe this is an important focus area, but since I’m not an SQL guru, I’ll leave it to others to comment on how this can best fit into the Plone experience. I have a related suggestion, though — one that we have been kicking around for quite a while at various sprints and workshops, but never had time to look into:

I want us to have a simple “Grid” type as one of the core Plone types. Looking at the average user, they usually reach for a spreadsheet not when they need calculation abilities — but when they need something that looks tabular. Week plans, project planning and overviews — all very structural data. Of course you can do this in a table in a normal HTML page, but that’s not how people think.

The Grid type wouldn’t even have to have any spreadsheet-like properties included — as long as it could deal with text input in tables and do some rudimentary operations — like import/export CSV, and possibly feed other parts of the system with the data. This would be the perfect back-end for things like polls, questionnaires and other similar applications. You could then export the data as CSV for specialised handling and analysis/graphing, if needed. Of course, using the [Chart API from Google] would be an easy way to provide an optional graphing capability if your data is public. But I assume I’m a bit biased in this area.

Whether this would be backed by something like SQLite with the ability to scale up to another SQL database as the storage later — or simply be a basic type — I’ll leave for the people with that kind of experience to decide. In any case, a Grid type would fill a real need in the current Plone line-up.

## #8: Unify similar concepts

Programmers often have a tendency to abstract away things until they are almost unrecognizable, to the great frustration of UI designers everywhere, of course. Making things too generic makes it hard for people to recognize patterns and makes it harder for them to let the tool guide them in what they want to do. However, the opposite tendency is also very much prevalent, and leads to a lot of redundant implementations, and confusion in what add-on products to use.

Building a unified back-end for some central application structures would make it easier to maintain an infrastructure that is well-tested, performance-audited and maintained. I won’t go too far into this, since it’s way outside my league, but some suggestions for common applications that could share APIs and implementations — but have different user interfaces to fulfill the specific needs — are:

*   Ratings, comments, forums, mailing lists
*   Blogs, feeds, mail-in support, mail-out support, newsletters

These could even ship with the core, but be deactivated by default. But I know I’m pushing it here — and I’m also tackling another element of this in a later point — so I’ll leave it as an exercise for the reader to help identify clusters of similar functionality.

## #9: Provide a fantastic page compositing story

At the moment, there are several page compositing add-ons available for Plone. None of them make for a fantastic experience in creating pages that are composed of other pages, listings and resources — but it’s one of the most important focus areas for Plone 4.0. If I had my way, the user would never have to think about abstract concepts like viewlets, portlets, listings and content — the approach would be the same for everything.

Like the unified installation area, this is highly complex and could fill a document by itself — but I wanted to have a placeholder here, so we can discuss the current approaches and where to go in Plone 4.0. Hopefully this will be where I get to spend most of my development time for the 4.0 release.

## #10: Content re-use is overrated — people like folderish

This is a personal pet peeve, but since I said I would include some confrontational issues for discussion… ;-)

There are absolutely user stories where content re-use is important, but those tend to be highly specialized cases, not the common case. Example: why isn’t an Event folderish? How about a Page? You can always find them later, and Plone’s built-in Link Integrity will stop you from doing stupid things like deleting content that is referenced elsewhere. There might be technical reasons why we’re not doing this — and if there is, I’d like to hear them.

*Note:* I’m not advocating using File field/widgets, which I agree are an abomination in most cases. I want to be able to add resources inside a content item, though. The simple reason is: that’s how people think, and we’ll have to build around that notion, at least for the basic functionality. Combined with our powerful search capabilities, this is a problem I’m confident we can provide an elegant solution for.

## #11: Take project workspaces seriously

Plone is an ideal platform for highly collaborative work, and we have been improving this story over the years. Organizations like OpenPlans have also done a lot of great exploratory work in this area. It’s time to bring some of that hard-earned knowledge into Plone proper. Using an approach similar to what [Martin Aspeli] has created with his b-org add-on, it should be possible to offer simple, compelling and efficient project workspaces without adding a lot of complexity or code.

## #12: Increase efforts on plone.org

The situation of plone.org is familiar to anyone that runs a successful company: Who has time to update and improve the company web site when business is doing so well? The same goes for the plone.org web site — most of our developers and integrators are busy and happy working on client projects. However, without a major facelift for our communal property, we aren’t doing Plone real justice.

Some things I believe we need to tackle are:

*   **Performance tuning** — currently, nobody has time to look into tuning the setup so it scales better. Currently, it’s a bit painful to use as a logged-in user because of the delays. This is to be expected with our growth, but that doesn’t mean we shouldn’t fix it.
*   **More functionality for community input** — we need ratings on everything, but especially add-on products.
*   **Identify documentation leaders** — at the documentation sprint, we put in place the framework and social notion of people having responsibility and oversight of certain areas of the documentation (e.g. LDAP integration). This needs to be put into action, and a consolidation effort for the documentation needs to happen as soon as possible.
*   **OpenID support everywhere** — yes, *especially* in things like the issue tracker. Make it easy for casual observers to comment and interact without creating new accounts. Since Yahoo, Google and others are currently adding OpenID support to various services, everyone will soon be able to use this.
*   **Sell Plone better** — The web site needs to sell Plone better, both in prose and design. The marketing committee and the Strategic Planning Summit are gearing up to address these issues.
*   **Make it easy to translate web site content** — This is on our more long-term radar, LinguaPlone could be used for this already, but it’s important to have a good workflow and process around this, so translations aren’t outdated when content changes. Expect it to take a while, but make sure you help out in finding the right balance of convenience and structure. Plone is successful on an international scale, our web site should be too!

## #13: Lower the Migration barriers from other systems

If we can make it easier to upgrade from other systems to Plone, our adoption will increase. Migration is a complex thing, and can never be 100% automated, but there are a number of things we can do to make it possible for people to receive the Plone goodness even if they started out on a different platform:

*   **A robust XML export/import strategy** — [GSXML] seems to be the best option I have seen so far, but figuring out what to standardize on, and have one solution that we recommend will make things much easier.
*   **Share migration scripts** — even if what you did was a horrible hack to get that Sharepoint content into Plone, share it! Over time, people can improve and contribute back to these scripts, and people who are considering a migration have some hard-won knowledge to start off with.
*   **Support common markup formats from other systems** — The classic case here is people who install MediaWiki, and then discover “you know, having a bit more granular security would be nice”. Having optional markup modules available so people can stick with what they know will increase the chances of them migrating upwards on the food chain.

## #14: Realize that web publishing isn’t our main arena

Another slightly controversial idea. I’m not saying that we should ignore simple web publishing — simply that it’s not an area we will ever be a dominant player in — and we’re not trying to be. A great static deployment story will help tremendously here, but there are several factors that will always make us more of a fringe player in the brochureware corporate web site / blog arena:

*   PHP systems will always win in pure numbers from pure widespread hosting availability.
*   Plone is massive overkill for a simple web site.
*   Software-as-a-Service providers like Google and others will also take on this area — and win.
*   It’s where the volume is, but not where money and talent is. We do money and talent better than volume. ;-)

In my opinion, we should focus our efforts in the areas where we are already doing really well:

*   Intranet deployments for companies and organizations.
*   Highly collaborative workspaces with sophisticated security requirements.
*   Document management.
*   …and numerous other specialized fields.

## #15: Let Favorites come back in a big way

Early versions of Plone had the notion of “Favorites” — a way to bookmark content that was useful to you in a particular Plone site. This implementation unfortunately was very basic, so it didn’t get much use among the power users, and it never got much maintenance or improvement. Later on, it was dropped, since nobody was willing to maintain it, and it really didn’t do much anyway.

I’m bringing sexy back — uhm, I mean…Favorites back. There are a number of great uses for this feature outside of a simple list of documents, as witnessed by many an inflated web startup over the last few years. I’d like us to bring back new, improved favorite support in Plone. Some ideas for cool things we can do with these:

*   Use Gmail’s “Star” concept to make an efficient, simple UI to mark content for later retrieval.
*   Dashboard portlets with your favorites.
*   Let the number of "stars" on a document imply things about its ranking in search results. As del.icio.us shows, this is a great indicator of popularity.
*   Introduce the "follow" concept from Twitter to Plone — “star” users like content — and be able to follow their updates and additions in the site. Dashboard portlet to list these, with RSS support.
*   The favorite marker can mark content to be available offline via plugins like [Google Gears]
*   …and I’m sure you have other great examples.

## #16: Improve desktop integration

Plone is already far ahead of its competitors in this area, thanks to the tireless efforts of Enfold Systems on the Windows side of things with Enfold Desktop. There’s still a lot of challenges here, and some specifics I’d like to discuss are:

*   Can we extend this kind of functionality to the OS X and Linux desktops?
*   Can we publish guidelines that show how to write your add-ons so they are friendly to WebDAV and desktop integration?
*   Can we make the Content Type Registries capable of having local configuration mappings, so they do the right thing depending on context — and make it easy to set up new mappings?
*   Can we make it easy to batch-apply metadata on upload using some sort of URL mapping?
*   Can we build a comprehensive test suite to make sure that our assumptions don’t break when we do changes in the various underlying implementations? Can we make it a given that a shipping Plone release has working WebDAV integration on all the major implementations — every time?

## #17: "Best apps" tier

This is more a pie-in-the-sky discussion item. Looking at other successful open source communities, it strikes me that Plone has a lot in common with how the [KDE project] manages their platform. They have a core that is useful in itself, but also a lot of add-ons that are more or less recommended — and the best-of-breed even ship with KDE itself. Working out how we can achieve a similar model with cues taken from the earlier point on installation capabilities and upgrade/dependency management would be very interesting.

There are a number of add-ons that deserve tighter integration (mostly in a political sense) with Plone — having recommended solutions for certain common add-ons helps the platform in general. We already have some projects that in some ways behave like this — LinguaPlone comes to mind — but having an accepted process and some guidelines around this is something we’ll need sooner or later.

## #18: Stay hungry & foolish

Plone has always been great at encouraging experimentation and wild ideas. I want us to continue doing this, and make it clear that we value it — but at the same time let people know that experiments aren’t necessarily going to end up in the core. Do more experimentation and stuff that has “fun” PR value — things like iPhone support and social network features are relevant contemporary examples.

* * *

What’s on your list? What are the things you’d like to see Plone focus on? Do you have comments or suggestions on any of the above points? [Leave a comment on the mailing list], or blog about your own list!

Thank you for your attention. It’s truly an exciting time in the Plone world.

[Plone Strategic Planning Summit]: http://plone.org/events/2008-summit
[Martin Aspeli]: http://martinaspeli.net
[Paul Everitt]: http://radio.weblogs.com/0116506/
[OpenPlans Team]: http://www.openplans.org
[Andreas Zeidler]: http://zitc.de/
[Plone4Artists]: http://plone4artists.org
[Andy McKay]: http://www.agmweb.ca/blog/andy/
[Mike Solomon]: http://www.culater.net/
[Tarek Ziade]: http://tarekziade.wordpress.com
[already started work on this]: http://tarekziade.wordpress.com/2008/01/21/snow-sprint-report-2-benchmarking/
[Chart API from Google]: http://code.google.com/apis/chart/
[Martin Aspeli]: http://martinaspeli.net
[GSXML]: http://plone.org/products/gsxml
[Google Gears]: http://gears.google.com/
[KDE project]: http://www.kde.org
[Leave a comment on the mailing list]: http://www.nabble.com/18-Things-I-Wish-Were-True-About-Plone-to15180006s6741.html
