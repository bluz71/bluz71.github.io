---
title: Simple Computing Security Tips
layout: default
comments: true
published: false
---

Simple Computing Security Tips
==============================

Being secure in this fast moving and ever changing digital world can be a
challenge, especially for those who are not IT professionals. Security is
usually the last thing folks want to deal with; *bad stuff won't happen to me*,
*devices and services are secure out of the box, right?*

You lock your doors for a reason, that real world logic also applies to the
digital world. There are many bad actors out there: nation-states,
cybercriminals, hackers all the way through to [script
kiddies](https://en.wikipedia.org/wiki/Script_kiddie) and
[wardrivers](https://en.wikipedia.org/wiki/Wardriving).

This is a guide of common-sense tips that most folks should be able to follow
to greatly improve their digital security.

Keep Up To Date
===============

**The** most important rule to enhance your digital security is to keep all
software and devices up to date. Security flaws are constantly being discovered
and fixed. [Unpatched flaws](https://www.networkworld.com/article/2926010/security0/the-unrelenting-danger-of-unpatched-computers.html)
are a real danger.

Modern software such as computer operating systems, browsers and apps by and
large automatically update themselves, or will notify you when updates are
available. If you are a Linux user please select a distribution with
long-term-support, I recommend [Linux Mint](https://linuxmint.com).

Please, never turn off automatic updates. Please be proactive in searching for
and applying these updates. Replace old software that does not automatically
update with a modern equivalent that does automatically update, an example
would be a PDF reader.

Hardware, such as routers or NASs, on the other hand usually do not update
themselves. It is equally important, if not more so, that these devices are
updated occasionally; a good rule is to update seasonally. Don't be afraid to
call in help if you are not able to update router or NAS firmware yourself.

Modern iPhones and iPads automatically update, which is excellent.

Android on the other hand is a mess; old phones will not receive updates, even
Android phones that are only a couple years old will likely not receive
security updates either. Your best bet with Android is to stick with Google
Pixel phones, these will receive security updates directly from Google itself
and will do so for multiple years.

Passwords
=========

Computers are getting faster and so is the ability for cybercriminals and
hackers to crack passwords. Most folks have poor password hygiene; passwords
that are too weak in combination with reusing passwords.

A six character password can be cracked in seconds on a modern password
cracking machine. Seven character passwords are also weak, same for eight and
nine character passwords.

Please watch [this video](https://www.youtube.com/watch?v=yzGzB-yYKcc) between
Edward Snowden and John Oliver explaining the importance of passwords.

My recommendation, passwords should be at least thirteen characters, preferably
much more. Use phrases to gain complexity, for instance
`boatsWillFloatPlanesWillGl1de!` is a **far** stronger password than
`ghU89!ghkQwl$` and is a heck of a lot easier to remember.

Also, the need to remember passwords results in users reusing passwords across
multiple services. This is
[very bad](http://www.zdnet.com/article/repeat-after-me-reusing-passwords-is-bad/).
It is strongly recommended to use unique passwords per context, your user
password should differ from your Google password which should also differ from
your Facebook password and so on.

If reusing passwords is bad then what can we do to remember all these passwords?

Use a [password manager](https://en.wikipedia.org/wiki/Password_manager). I
prefer locally installed password managers such as
[KeePass](https://keepass.info) or one of its derivatives such as
[KeePassXC](https://keepassxc.org) (this is my preferred password manager).
These, compatible, password managers store your passwords in an encrypted
database protected by one master password. Please choose a very strong master
password. Also when using a locally installed password manager, such as
KeePassXC, please keep a copy of the encrypted database, usually
`Passwords.kdbx`, safely stored offsite, I recommend a CD-R stored in a safety
deposit box.

I suggest storing all your accounts and passwords in KeePassXC; when a new
password is needed let KeePassXC generate one for you. With experience using a
password manager will become second nature whilst at the same time it will
**greatly** improve your digital security.

Smartphone PINs
===============

TODO

Phishing Scams
==============

[Phishing](https://en.wikipedia.org/wiki/Phishing) is the attempt to obtain
sensitive information such as usernames, passwords, and credit card details
(and, indirectly, money), often for malicious reasons, by disguising as a
trustworthy entity in an electronic communication.

The most common form of phishing is unsolicited email disguised as coming from
a legitimate entity you may have an account with. For example it may be an
email styled to look like it came from PayPal with a link to a fake PayPal
site designed to fool the victim into typing in their username and password.

To protect yourself from such scams please be sceptical of any unsolicited
correspondence, either in email or messaging form. Do **NOT** click links, do
**NOT** open attachments, do **NOT** be tricked into sending your password in
electronic form. Do not trust links in emails, they may be scam.

When required to, log directly into the official portal of a service provider,
such as a bank or ecommerce vender, via a browser bookmark or by typing the web
address into a browser.

If you receive such an email, delete it immediately.

Social Engineering Scams
========================

[Social engineering](https://en.wikipedia.org/wiki/Social_engineering_(security))
is the manipulation of people into divulging confidential information.

The most common form of computer-related social engineering are unsolicited
phone calls from entities claiming to be a bank or a telecommunications
provider that require your personal details to solve an issue.

To protect yourself from these scams please **never** provide your confidential
details to **anyone ever**. No one but you should know your confidential
information such as an online bank password. Note, a real bank will not ring
you asking for your password.

If you receive such a call, please hang up immediately.

Malicious USB Sticks
====================

A trick used by some hackers is to drop malicious USB sticks in public places
ready for unsuspecting folk to pick them up, take them home, and insert them
into their computer. Such malicious USB sticks, once inserted, can then
seriously compromise your network security.

Your should
[never trust a found USB stick](https://threatpost.com/never-trust-a-found-usb-drive-black-hat-demo-shows-why/119653/).

If you see a USB stick on the ground, simply leave it there and walk on.

Public Wi-Fi, Be Very Cautious
==============================

Using free public Wi-Fi can be
[very dangerous](https://www.goldenfrog.com/blog/the-dangers-of-public-wi-fi).
Using free Wi-Fi opens you up
to: [man-in-the-middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)
attacks,
[snooping & sniffing](https://security.stackexchange.com/questions/52980/sniffing-snooping-spoofing)
, and
[malicious hotspot](https://www.howtogeek.com/178696/why-using-a-public-wi-fi-network-can-be-dangerous-even-when-accessing-encrypted-websites/)
attack.

It is strongly recommended that you use the data bandwidth associated with
your mobile phone to access the internet when on the go. Tethering a laptop to
a mobile phone is not difficult these days.

If your data bandwidth is limited, and you simply **must** use public Wi-Fi, then
it strongly recommended that you only use such Wi-Fi in combination with a
[strong VPN](http://www.techradar.com/news/public-wi-fi-and-why-you-need-a-vpn).
I recommend a trusted commercial non-logging VPN provider such as:
[ProtonVPN](https://protonvpn.com), [BolehVPN](https://www.bolehvpn.net) or
[IVPN](https://www.ivpn.net). If you do not wish to pay for a VPN service then
simply use ProtonVPN free VPN service.

Disk Encryption
===============

[Disk encryption](https://en.wikipedia.org/wiki/Disk_encryption) is technology
that protects your data, stored on disk, by scrambling it when devices are
turned off. For example, disk encryption will protect the confidentiality of
your data if your lose a powered off laptop, or if a thief steals your home
computer.

Disk encryption requires a master password, hence it is very important that you
select a strong password as your master, at least fifteen characters long.
Also, please never forget this password, without it you will be locked out of
your own data.

Computers are so fast these days that it makes sense to enable disk encryption
on all computers and devices.

* Windows users are encrouraged to upgrade to the *Pro* edition and then enable
  [Bitlocker](https://en.wikipedia.org/wiki/BitLocker) full disk encryption. The
  cost upgrade to *Pro* is unfortunate, but it is a small price to pay for piece
  of mind and security.

* Mac ships with [FileVault](https://en.wikipedia.org/wiki/FileVault) disk
  encryption. However, it is not enabled by default, hence you must enable it
  in *Security & Privacy* settings.

* Linux distributions provide [LUKS](https://en.wikipedia.org/wiki/Linux_Unified_Key_Setup)
  disk encryption. Please enable full disk encryption at installation time.

* iPhones and iPads, which both use the iOS operation, encrypt all data by
  default. Please also set a strong PIN code.

* Modern Android phones encrypt by default whilst most older Android phones do
  not. Old Android phones are insecure, hence it is recommened to upgrade to a
  modern Android device such as a Google Pixel phone. Please also set a strong
  PIN code.

* Modern NASs, such as QNAP and Synology devices, also provide encryption
  capabilities. If a NAS is used to store confidential information, such as
  financial details or family photos, then it is recommended to setup encrypted
  shares. Just be aware you will need to log into the management interface to
  unlock a share each time it is rebooted or restarted; a very minor
  inconvenience.

Browser Recommendations
=======================

For all platforms, apart from iPhones and iPads, I recommend the Google Chrome
browser for its enhanced security. From sandboxing, to hardened builds, to
auto-updates, to multi-platform support, Chrome is hard to beat. For iPhones
and iPads stick with the default Safari browser.

Chrome security can be strengthened by the addition of these three excellent
extensions:

* [HTTPS Everywhere](https://www.eff.org/https-everywhere) is an extension that
  will force the usage of encryption wherever possible. Certain web sites offer
  both plain-text and encrypted connections but will default to plain-text,
  these connections can be evesdropped. This extension will force the secure
  connection in such circumstances.

* [uBlock Origin](https://chrome.google.com/webstore/detail/ublock-origin/cjpalhdlnbpafiamejdnhcphjbkeiagm?hl=en)
  is a content filtering extension most well known for blocking ads, but it
  also blocks content from sites known to host malware (malicious software).

* [Privacy Badger](https://chrome.google.com/webstore/detail/privacy-badger/pkehgijcmpdhfbdbbnkijodmdjhbjlgp)
  is an extension that learns to block trackers as you browse the web. It will
  increase the privacy and security of your session.

Secure Your Router and Wi-Fi
============================

Weak router and Wi-Fi configuration is a common source of network insecurity,
it can allow hackers from the other side of the world, or wardrivers sitting
outside your house into your home network.

Configuring a rourter can be daunting for those not experienced in technology.
Modern routers are slightly less obtuse than they used to be. Again, don't be
afraid to call in help, better that than living with an insecure router.

These simple rules will greatly increase the security of your router:

* Change the default administror password of your router. This should be done
  **immediately**. As per-usual choose a very strong password.

* Please make sure that your router's Firewall is enabled (sometimes noted as
  SPI). With most modern routers this will be enabled by default.

* When setting up Wi-Fi please active strong encryption, that means
  configuring *WPA2 Personal* (sometimes noted as *WPA2-PSK*) only.

* Select a unique Wi-Fi identifier (SSID), do not use a default identifier.

* Lastly, choose an **extremely strong** Wi-Fi password, at least thirty
  characters long is desired.

Back it Up
==========

Offsite and Apricorn drive

Install Only from Trusted Sources
=================================

Chrome Browser
==============

Smartphone
==========

Windows
=======

Stop using old versions of Windows
Don't like most A/V
Use MalwareBytes

Mac
===

Linux
=====

rkhunter
