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
usually the last thing folks want to deal with, bad stuff won't happen to me,
devices and services are secure out of the box, right?

You lock your doors for a reason, that same logic applies to the digital world.
There are many bad actors out there: nation-states, cybercriminals,
hackers all the way throught to
[script kiddies](https://en.wikipedia.org/wiki/Script_kiddie) and
[wardrivers](https://en.wikipedia.org/wiki/Wardriving).

This is a guide of my common-sense tips that most folks should be able to
follow to greatly improve their digital security.

Keep Up To Date
===============

**The** most important policy to enhance your digital security is to keep all
software and devices up to date. Security flaws are constantly being
discovered and fixed.

Critical software such as computer operating systems, browsers and apps by and
large now automatically update. Please, never turn off these automatic updates.

Hardware, such as routers, on the otherhand usually do not update themselves.
It is equally important, if not more important, that these devices are updated
occasionally; a good rule is to update seasonally. Don't be afraid to call in
help if you are not able to update router firmware.

Talk about smartphones.

Passwords
=========

Computers are getting faster and so is the ability for cybercriminals and
hackers to crack password. Most folks have poor password hygiene; passwords too
weak in combination with password reuse.

A six character password can be cracked in seconds on a modern password
cracking machine. Seven characters is also weak, same for eight and nine.

My recommendation, passwords should be at least twelve characters, preferably
more. Use phrases to gain complexity, for instance
*boatsWillFloatPlanesWillGlide669!* is a **far** stronger password than
*ghU89!ghkQwl$* and is a heck of a lot easier to remember.

The need to remember passwords results in users reusing passwords. This is
dangerous. It is strongly recommended to use unique passwords per context; you
laptop login password should differ from your Google password which should also
differ from your Facebook password and so on.

How to remember all these passwords?

Use a password manager, only one password to remember. I prefer offline
password managers such as [KeePass](https://keepass.info) and its derivatives
such as [KeePassXC](https://keepassxc.org). These, compatible, password managers store
your passwords in an encrypted database protected by one master
password. Please choose a very strong password for the master. Also, keep a
copy of the KeePass database, usually `Database.kdb`, safely stored offsite, I
recommend a CD-R stored in a safety deposit box.

Phishing Scams
==============

[Phishing](https://en.wikipedia.org/wiki/Phishing), as noted at Wikipedia, is
the attempt to obtain sensitive information such as usernames, passwords, and
credit card details (and, indirectly, money), often for malicious reasons, by
disguising as a trustworthy entity in an electronic communication.

The common form is unsolicited email disguised as being from a legitimate
entity. For example it may be an email styled to look like it came from *PayPal*
with a link to a fake PayPal site designed to fool the victim into typing in
their username and password.

To protect yourself from such scams please be sceptical of any unsolicited
correspondence, either in email or messaging form. Do *NOT* click links, do
*NOT* open attachments. Do not be tricked into sending your password in
electronic form.

Always log in to the official portal of a service provider, such as a bank or
ecommerce vender, via a bookmark. Never click a link in a message claiming to
be an entity.

Social Engineering Scams
========================

[Social engineering](https://en.wikipedia.org/wiki/Social_engineering_(security)),
as noted at Wikipedia, refers to psychological manipulation of people into
performing actions or divulging confidential information.

The common form of information technology social engineering would be
unsolicited phone calls from entities claiming to be official entites (for
example a bank, or a telecommunications provider) that requires your personal
details to solve an issue.

To protect yourself from these scams please never provide your confidential
details to anyone. No one but you should know your confidential credentials.
Real banks will never ring you asking for your password.

Keep your passwords confidential always.

Malicious USB Sticks
====================

A trick used by some hackers is to drop malicious USB sticks in public places
ready for unsuspecting folk to pick them up, take them home, and insert them
into their computer. Such USB sticks, once insertead, can then compromise your
digital security.

Your should
[never trust a found USB stick](https://threatpost.com/never-trust-a-found-usb-drive-black-hat-demo-shows-why/119653/).

If you see a USB stick on the ground, simply leave it there and walk on.

Public Wi-Fi, Be Cautious
=========================

Using free public Wi-Fi can be
[very dangerous](https://www.goldenfrog.com/blog/the-dangers-of-public-wi-fi).
Using such free Wi-Fi opens you up
to: [man-in-the-middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)
attacks,
[snooping & sniffing](https://security.stackexchange.com/questions/52980/sniffing-snooping-spoofing)
, and
[malicious hotspot](https://www.howtogeek.com/178696/why-using-a-public-wi-fi-network-can-be-dangerous-even-when-accessing-encrypted-websites/)
attack.

It is strongly recommended that you use that data bandwidth associated with
your mobile phone to access the internet when on the go. Tethering a laptop to
a mobile phone is not too difficult these days.

If your data bandwidth is limited, and you **must** use free public Wi-Fi, then
it strongly recommended that you only use such Wi-Fi in combination with a
[strong VPN](http://www.techradar.com/news/public-wi-fi-and-why-you-need-a-vpn).
[Here](XXX) are my VPN provider recommendations.

Disk Encryption
===============

Disk encryption is technology that protects your data, at rest, by scrambling it when
devices are turned off. For example, disk encryption will protect the
confidentiality of your data if your lose a powered off laptop, or a thief
steals your home computer.

Disk encryption requires a master password, hence it is very important that you
select a strong password as your master, at least 15 characters long (if not
more). Also, please never forget this password since without it you will be
locked out of your own data.

Computers are so fast these days that it makes sense to enable disc encryption
on all your computers and devices.

* Windows users are encrouraged to upgrade to the *Pro* edition and then enable
[Bitlocker](https://en.wikipedia.org/wiki/BitLocker) full disk encryption. The
cost upgrade to *Pro* is sad, but it is a small price to pay for piece of mind.

* Mac ships with [FileVault](https://en.wikipedia.org/wiki/FileVault) disk
encryption. However, it is not enabled by default, hence you must enabled it in
*Security & Privacy* settings.

* Linux distributions provide [LUKS](https://en.wikipedia.org/wiki/Linux_Unified_Key_Setup)
disk encryption. The easiest approach in Linux is to enable full disk
encryption at installation time.

* iPhones and iPads, which both use the iOS operation, encrypt all data by default. However,
please make sure you have a strong PIN setup, at least 8 characters, along with
TouchID for fast unlike.

* Android, modern Android phones encrypt by default, however, nearly all Android phones
older than two years old are unlikely to encrypt by default. Old Android phones are
a security disaster due to a lack of security updates. My advice, replace old
Android phones with a modern smartphone. Again, it is recommended that a strong
PIN code be enabled, at least 8 characters along with finger print unlock, if
your phone supports it, for fast unlock.

* NAS, todo

Lock Down Your Network
======================

Router password.
Enable firewall.
Use strong WiFi

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
