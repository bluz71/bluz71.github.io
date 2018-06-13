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
due to strict browsing mode invoked, and bandwidth contraints due to the basic
architecture of the network. However, I do recommend Tor for whistleblowers
(see below).

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

Browser Setup
=============

SMS and Emails are Not Private
==============================

Use a European email Provider
=============================

Signal and WhatsApp messaging
=============================

Google Setup
============

Google alternative
==================

Facebook Setup
==============

Windows Privacy
===============

Don't trust.
Disable telemetry

Linux Privacy
=============

Custom kernel
rkhunter
lynis

MAC Privacy
===========

https://github.com/drduh/macOS-Security-and-Privacy-Guide

Cover your Webcam
=================

Signal blocking bag for phone
=============================

Whistleblowing
==============

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

