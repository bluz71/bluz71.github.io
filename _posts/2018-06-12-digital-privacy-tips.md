---
title: Digital Privacy Tips
layout: default
comments: true
published: false
---

Digital Privacy Tips
====================

We now live in an age where everyone in the digital world is being surveilled,
profiled and targeted. That may be at a nation-state level, such as by the
American [NSA](https://en.wikipedia.org/wiki/National_Security_Agency) and
British
[GCHQ](https://en.wikipedia.org/wiki/Government_Communications_Headquarters)
with their mass-surveillance systems, or it may be at a private enterprise
level, such as by the internet giants Google and Facebooks with their
targeting advertising systems.

Digital privacy is the right to protect your personal information from:
unpermitted collection, unpermitted use, and unpermitted distribution to third
parties.

This guide contains tips and suggestions that readers can use to increase their
privacy in the digital domain.

Note, if you are genuine target of a nation-state then realistically there is
little you can do to remain private, this guide will not be of much use. But
most citizens do not fall into that category.

Security
========

Before tackling digital privacy it is **extremely** important that one has a
strong security regime in place. Please read the
[Sensible Computing Security
Tips](http://localhost:4000/2017/11/12/sensible-computing-security-tips.html)
as a starting point to secure you digital devices.

You can not be private unless you are first secure. Please keep up-to-date,
enable disk encryption, use strong unique passwords and don't be a victim of
phishing and social engineering.

ISP Snooping and Logging
========================

Depending in which country you live in, and possibly which provider you use,
your internet activity may be snooped and logged by your internet service
provider (ISP). That may include the following information: which sites you
visit, who you communicate with, at what time, and for how long to name a few.

That information may also be mirrored by intelligence agencies to feed into
their [mass-surveillance systems](https://en.wikipedia.org/wiki/Stellar_Wind),
or the ISP may sell that information to advertisers to [target ads to
you](https://www.wired.com/2014/10/verizons-perma-cookie). It is even possible
that cyber-criminals may steal that information for their own nefarious means.
Basically if your activity is logged, it will be a target for various bad
actors.

The most appropriate technology to circumvent ISP snooping and logging is a
[virtual private
network](https://en.wikipedia.org/wiki/Virtual_private_network) (VPN). A VPN
creates a private network across a public network, most often the internet. A
proper virtual network creates a secure encrypted tunnel to your VPN provider's
[data center](https://en.wikipedia.org/wiki/Data_center) and then onto the
internet thus blinding your ISP to your activity. Note, when using a VPN you
are shifting your trust relationship from the ISP to the VPN provider, hence it
is important that you select a trust-worthy commercial provider that respects
your privacy and is secure.

Quality VPN providers I trust:

- [IVPN](https://www.ivpn.net)

- [ProntonVPN](https://protonvpn.com)

- [BolehVPN](https://www.bolehvpn.net)

- [Mullvad](https://mullvad.net/en)

[This Wirecutter](https://thewirecutter.com/reviews/best-vpn-service) article
offers useful information on the subject of VPNs.

An alternative to using a VPN is the
[Tor](https://en.wikipedia.org/wiki/Tor_(anonymity_network)) anonymity network.
For most people I do **not** recommend Tor for a number of reasons including:
poor performance due to the multiple network hops involved, poor web experience
due to the strict browsing mode used, and bandwidth contraints due to the basic
architecture of the network. A VPN will provide a superior user experience.

DNS
===

The [domain name system](https://en.wikipedia.org/wiki/Domain_Name_System)
(DNS) is effectively a phone book for computer addresses. DNS translates the
`www.google.com` human-readable addresses to something a computing device can
understand such as `172.217.22.4`.

The proprietor of a DNS end-point has great power, they can log your activity
and they can block or censor your activity

Most people likely use the DNS servers provided by their ISP. If privacy is a
desire then it is recommended you change your DNS configuration to an
unfiltered DNS server.

Note, if you are using a quality VPN service, as listed above, then you will be
using the DNS provided by the VPN provider, which is good. If you not connected
to a VPN then recommend the [Neustar Free Recursive DNS
servers](https://www.security.neustar/dns-services/recursive-dns) (previously
known as DNS Advantage) be configured in your router.

```
IPv4 156.154.70.1, 156.154.71.1
IPv6: 2610:a1:1018::1, 2610:a1:1019::1
```

Note, Google offers fine DNS servers at `8.8.8.8` and `8.8.4.4`, but I
recommend against the use of these servers if privacy is a prime concern.

Browser Privacy
===============

In the [Sensible Computing Security
Tips](http://localhost:4000/2017/11/12/sensible-computing-security-tips.html#browser)
post I recommended the use of Google Chrome with the following extensions:

* [HTTPS Everywhere](https://www.eff.org/https-everywhere)

* [uBlock Origin](https://chrome.google.com/webstore/detail/ublock-origin/cjpalhdlnbpafiamejdnhcphjbkeiagm?hl=en)

* [Privacy Badger](https://chrome.google.com/webstore/detail/privacy-badger/pkehgijcmpdhfbdbbnkijodmdjhbjlgp)

That combination of browser and extensions increases both security **and**
privacy, they latter by the blocking of many [web
trackers](https://whatismyipaddress.com/web-tracking).

Privacy can further be enhanced by tweaking the following Chrome settings.

- In *Privacy and Security*, enable `Send a "Do Not Track" request with
    your browsing traffic`

- Disable `Use a web service to help resolve navigation errors`

- Disable `Use a prediction service to help complete searches and URLs typed in
    the address bar`

- Disable `Use a prediction service to load pages more quickly`

- In *Content Settings / Cookies*, enable `Keep local data only until you quit
    your browser`

- Enable `Block third-party cookies`

Note, the second to last cookie setting will clear your session cookies upon
browser exit. Session cookies can be used to track and de-anonymize users, by
clearing them you increase your privacy at the expense of more frequest
re-login into web services. This latter negative is not that onerous with
cached user credentials.

Email
=====

[Email](https://en.wikipedia.org/wiki/Email) is one of the most common methods
of electronic message exchange in use today. Email is an old technology that
pre-dates the Internet. Privacy was never a consideration in its design, and to
this date still has not been satisfactorily addressed, and likely never will
be.

Note, I am ignoring the use PGP in Email because it is complicated,
error-prone, and no longer recommended for use by [leading
cryptographers](https://blog.cryptographyengineering.com/2014/08/13/whats-matter-with-pgp).

Many users incorrectly assume that an email exchange is a private
correspondence. In reality it is more akin to a loud conservation in public. At
a minimum the Email provider has access to all the
[plain-text](https://en.wikipedia.org/wiki/Plaintext) content. Cyber-criminals
through to mass-surveillance systems may gain access to this content and
exploit it as happened to [John Podesta in
2016](https://en.wikipedia.org/wiki/Podesta_emails).

One issue is that some users never delete their old emails from their
provider's system. In John Podesta's case that was 60,000 emails and a world of
hurt. Please consider deleting old emails, how often do you read emails from
five years ago?
 
Another note of caution, be careful what you say in email, only be prepared to
write what you a comfortable with if, in the unlikely event,
[Wikileaks](https://wikileaks.org/) were to publish it.

When it comes to mass-surveillance systems the jurisdiction of the provider is
a factor that should be considered. For instance, Google's
[Gmail](https://www.google.com/gmail/) is an excellent Email service, but it
can not be considered privacy-focussed because Google is a partner in the
mass-surveillance
[PRISM](https://en.wikipedia.org/wiki/PRISM_(surveillance_program)) program.

These excellent European-based Email providers do respect privacy.

- [ProtonMail](https://protonmail.com)

- [Runbox](https://runbox.com)

- [Posteo.de](https://posteo.de)

Email still has use today due to its simplicity and ubiquity, but when it comes
to privacy please consider using the Signal application detailed next.

Signal Application
==================

The [Signal](https://signal.org) application is a modern
[end-to-end](https://en.wikipedia.org/wiki/End-to-end_principle) secure
messaging application primarily used on
[Android](https://en.wikipedia.org/wiki/Android_(operating_system)) and
[iOS](https://en.wikipedia.org/wiki/IOS) devices.

The [openly developed](https://github.com/signalapp) Signal application uses
unobstrusive best-in-class cryptography housed in an extremely easy to use
interface that anyone, be they technical or non-technical, can use. Text,
voice, photos can securely be exchanged between two or more recipients. 

Content is encrypted prior to sending and can only decrypted by the intended
recipient. The servers used to host the Signal service do **not** have access
to the [plain-text](https://en.wikipedia.org/wiki/Plaintext) of a conversation,
not even the [FBI is able to gain
access](https://arstechnica.com/tech-policy/2016/10/fbi-demands-signal-user-data-but-theres-not-much-to-hand-over).

Android users can make Signal their default SMS application, and one
by one convince the friends to also adopt Signal. One such inducement to change is
that [SMS is not a secure or private
technology](https://www.rokacom.com/are-text-messages-encrypted).

The Signal application also supports [dissappearing
messages](https://support.signal.org/hc/en-us/articles/213134237-Does-Signal-have-disappearing-messages-).
This feature ensures that messages will be removed from your device and the
device of your recipient a chosen amount of time after they have read the
message. This is **especially** important for highly sensistive messages.

If you use a smart phone, and you care about messaging privately, then you and
your contacts should be using Signal.

Google
======

Google has more than [one
billion](https://techcrunch.com/2016/02/01/gmail-now-has-more-than-1b-monthly-active-users)
active users. Google's motto at one time was *don't be evil*, but they never said
anything abot respecting a user's privacy. If an internet service is free, like
Google, then **you** are the product. In Google's case they generate income by
targetting ads specifically to you. They want to know who you are and what you
do, and to figure this out Google gobbles up lots of user data.

To better target their ads Google will record the following details.

- Your search queries

- The search results you click on

- The IP address you use

- The location, and movement, of your Google-linked devices

- And likely a lot of other [metadata](https://en.wikipedia.org/wiki/Metadata)

For Android, YouTube and Gmail users it is simply not practical to delete their
Google account. However, there are *some* options are user can tweak to
decrease the amount of data Google collects about you.

Log into to [My Activity](https://myactivity.google.com). This lists all the
activity Google records for your account. Delete all the archives you no longer
want Google to store. Select the *Activity Controls* and `pause` all the
activities provided. Congratulations, Google is now collecting less of your
private information.

Web Searching
=============

The above Google changes does **not** entirely free you from the Google
ad-targetting monolith. Google will still record your queries, even if they say
otherwise.

True private web searching requires the use of an alternate search service.

Privacy-respecting alternatives to Google search.

- [StartPage](https://www.startpage.com) (uses Google search results under the
    covers)

- [DuckDuckGo](https://duckduckgo.com)

- [Quant](https://www.qwant.com)

Facebook
========

Cambridge Analytica

https://www.michaelzimmer.org/2010/05/14/facebooks-zuckerberg-having-two-identities-for-yourself-is-an-example-of-a-lack-of-integrity/

Facebook Beacon: https://en.wikipedia.org/wiki/Facebook_Beacon

Like buttons

Oversharing


Windows Privacy
===============

https://www.eff.org/deeplinks/2016/08/windows-10-microsoft-blatantly-disregards-user-choice-and-privacy-deep-dive
Don't trust.
Disable telemetry

MAC Privacy
===========

https://github.com/drduh/macOS-Security-and-Privacy-Guide

Cover your Webcam
=================

Signal blocking bag for phone
=============================

Disable GPS (it is a location tracker)

No Alexa, Voice Assistant, Webcam
=================================

Dissidents, Journalists & Whistle-blowers
=========================================

Summary
=======

The right to privacy is a basic human right, not a privilege, and that same
right should also apply in the digital domain.

Some would say, *but I have nothing to hide*, to those people I provide the
following quotes.

[Benjamin Franklin](https://en.wikipedia.org/wiki/Benjamin_Franklin) (one of the Founding Fathers of the United States)

> Those who would give up essential liberty, to purchase a little temporary
> safety, deserve neither liberty nor safety.

[Joseph Goebbels](https://en.wikipedia.org/wiki/Joseph_Goebbels) (Nazi Party)

> If you have nothing to hide, you have nothing fear

[Edward Snowden](https://en.wikipedia.org/wiki/Edward_Snowden) (Whistleblower)

> Arguing that you don't care about the right to privacy because you have
> nothing to hide is no different than saying you don't care about free speech
> because you have nothing to say

