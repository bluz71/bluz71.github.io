---
title: Sensible Computing Security Tips
layout: default
comments: true
published: true
---

Sensible Computing Security Tips
===============================

Being secure in this fast moving and ever changing digital world can be a
challenge, especially for those who are not IT professionals. Security is
usually the last thing folks want to deal with; *bad stuff won't happen to me*,
*devices and services are secure out of the box, right?*

You lock your doors for a reason, that real world logic also applies to the
digital world. There are many bad actors out there: nation-states,
cybercriminals, and hackers all the way through to [script
kiddies](https://en.wikipedia.org/wiki/Script_kiddie) and
[wardrivers](https://en.wikipedia.org/wiki/Wardriving).

This is a guide of common-sense tips that most folks should be able to follow
to greatly improve their digital security.

Keep Up To Date
===============

**The** most important rule to enhance your digital security is to keep all
software and devices up to date. Security flaws are constantly being discovered
and fixed,
[unpatched flaws](https://www.networkworld.com/article/2926010/security0/the-unrelenting-danger-of-unpatched-computers.html)
are a real danger. Running
[end of life software](https://www.spiceworks.com/it-articles/end-of-life-software-dangers/)
is also very dangerous. Cybercriminals use these known vulnerabilities to
target the unsuspecting.

Modern software such as operating systems, browsers and smartphone apps by and
large automatically update themselves, or will notify you when updates are
available.

Please, never turn off automatic updates. Please be proactive in searching for
and applying these updates. Replace old software that does not automatically
update with a modern equivalent that does automatically update, an example
would be an old PDF reader.

* Microsoft Windows users should immediately upgrade to the latest version,
  which is Windows 10 at the time of this post. Please **stop** using Windows
  7, Vista and XP; these versions are out-of-date, obsolete and insecure.

* Apple Mac upgrades are free, hence there is no reason not to upgrade to the
  latest release, which is High Sierra at the time of this post. Security
  updates will usually be made available monthly, please apply them immediately
  when prompted.

* Linux users should select a distribution that provides long term support and
  prompt security updates, I recommend [Linux Mint](https://linuxmint.com).

* Hardware, such as routers or NASs, usually do not update themselves. It is
  equally important, if not more so, that these devices are updated
  occasionally; a good rule is to update seasonally. Don't be afraid to call in
  help if you are not able to update router or NAS firmware yourself.

* Modern iOS devices, such as iPhones and iPads, automatically update.

* Android security updates are a mess; old phones do not receive updates, even
  Android phones that are only a couple years old are liable to not receive
  security updates either. Your best bet with Android is to stick with modern
  Google Pixel phones, these will receive security updates directly from Google
  itself and will do so for multiple years.

<a id="passwords"></a>Passwords and PINs
========================================

Computers are getting faster and so is the ability for cybercriminals and
hackers to crack user passwords. Most folks have poor password hygiene;
passwords that are weak in combination with password reuse.

A six character password can be cracked in seconds on a modern password
cracking machine. Seven character passwords are also weak, same for eight and
nine character passwords.

Please watch [this exchange](https://www.youtube.com/watch?v=yzGzB-yYKcc)
between Edward Snowden and John Oliver explaining the importance of passwords.

My recommendation, passwords should be at least fifteen characters long,
preferably more. Use phrases to gain complexity, for instance
`boatsWillFloatPlanesWillGl1de!` is a **far** stronger password than
`ghU89!yhkQwl$` and is a heck of a lot easier to remember.

Also, the need to remember passwords results in users reusing passwords across
multiple services. This is
[very bad](http://www.zdnet.com/article/repeat-after-me-reusing-passwords-is-bad/).
It is strongly recommended to use unique passwords per context, your laptop
user password should differ from your Google password which should also differ
from your Facebook password and so on.

If reusing passwords is bad then what can we do to remember all these long
passwords?

Use a [password manager](https://en.wikipedia.org/wiki/Password_manager).

I **much** prefer locally installed password managers such as
[KeePass](https://keepass.info) or one of its derivatives such as
[KeePassXC](https://keepassxc.org) (this is my password manager of choice).
These, compatible, password managers store your passwords in an encrypted
database protected by a master password. Please choose a very strong master
password. Also, when using a locally installed password manager, such as
KeePassXC, please keep a copy of the encrypted database, usually
`Passwords.kdbx`, safely stored offsite, I recommend a CD-R in a safety
deposit box.

I suggest storing all your accounts and passwords in KeePassXC; when a new
password is needed let KeePassXC generate one for you. With experience using a
password manager will become second nature whilst at the same time it will
greatly improve your digital security.

With smartphones I recommend PIN codes that are at least eight digits long.
Also configure fingerprint unlock, Touch ID on iPhones, if available, for fast
unlock. Long PINs will then not be that bothersome.

Lastly, please configure all your devices to auto-lock themselves after a
period of inactivity. Five minutes for computers and two minutes for
smartphones are reasonable choices.

Phishing Scams
==============

[Phishing](https://en.wikipedia.org/wiki/Phishing) is the attempt to obtain
sensitive information such as usernames, passwords, and credit card details,
often for malicious reasons, by disguising as a trustworthy entity in an
electronic communication.

The most common form of phishing is unsolicited email disguised as coming from
a legitimate entity you may have an account with. For example it may be an
email styled to look like it came from PayPal with a link to a fake PayPal
site designed to fool the victim into typing in their username and password.

To protect yourself from such scams please be sceptical of any unsolicited
correspondence, either in email or messaging form. Do **NOT** click links, do
**NOT** open attachments, do **NOT** be tricked into sending your password in
electronic form. Do not trust links in emails, they may be a scam.

When required to, log directly into the official portal of a service provider,
such as a bank or ecommerce vendor, via a browser bookmark or by typing the web
address directly into a browser.

If you receive such phishing email, delete it immediately.

Social Engineering Scams
========================

[Social engineering](https://en.wikipedia.org/wiki/Social_engineering_(security))
is the manipulation of people into divulging confidential information through
conversation.

The most common form of computer-related social engineering are unsolicited
phone calls from persons claiming to represent an entity, say a bank or
telecommunications provider, who then request your personal details to solve
an issue of sorts. These cold calls are never real, all they are after are your
credentials for nefarious purposes, usually to steal money.

To protect yourself from these scams please **never** provide your confidential
details to anyone **ever**. No one but you should know your confidential
information such as an online banking password. Note, a real bank will not ring
you asking for your password.

If you receive such a call, please hang up immediately.

Malicious USB Sticks
====================

A trick used by certain cybercriminals is the dropping of malicious USB sticks
in public places ready for unsuspecting folk to pick them up, take them home,
and insert them into their computer. Such malicious USB sticks, once inserted,
will compromise your home network.

Your should
[never trust a found USB stick](https://threatpost.com/never-trust-a-found-usb-drive-black-hat-demo-shows-why/119653/).

If you see a USB stick on the ground, simply leave it there and walk on.

Public Wi-Fi, Be Very Cautious
==============================

Using free public Wi-Fi can be
[very dangerous](https://www.goldenfrog.com/blog/the-dangers-of-public-wi-fi).
Free Wi-Fi opens you up
to: [man-in-the-middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)
attack,
[snooping & sniffing](https://security.stackexchange.com/questions/52980/sniffing-snooping-spoofing)
, and
[malicious hotspot](https://www.howtogeek.com/178696/why-using-a-public-wi-fi-network-can-be-dangerous-even-when-accessing-encrypted-websites/)
attack.

It is strongly recommended that you use the data bandwidth associated with
your mobile phone to access the internet when on the go. Tethering a laptop to
a smartphone is not difficult these days.

If your data bandwidth is limited, and you simply **must** use public Wi-Fi, then
it is strongly recommended that you only use such Wi-Fi in combination with a
[strong virtual private network (VPN)](http://www.techradar.com/news/public-wi-fi-and-why-you-need-a-vpn).
I recommend trusted commercial VPN providers such as:
[ProtonVPN](https://protonvpn.com), [BolehVPN](https://www.bolehvpn.net) or
[IVPN](https://www.ivpn.net). Note, ProtonVPN also offers a low-bandwidth free
VPN option, this is an excellent choice if you infrequently need to use public
Wi-Fi.

Disk Encryption
===============

[Disk encryption](https://en.wikipedia.org/wiki/Disk_encryption) is technology
that protects your data, stored on disk, by scrambling it when devices are
powered off. For example, disk encryption will protect the confidentiality of
your data if your lose a powered off laptop, or if a thief steals your home
computer.

Disk encryption requires a master password, hence it is very important that you
select a strong password as your master, at least fifteen characters long.
Also, please never forget this password, without it you will be locked out of
your own data.

* Windows users are encouraged to upgrade to the *Pro* edition and then enable
  [Bitlocker](https://en.wikipedia.org/wiki/BitLocker) full disk encryption. The
  cost upgrade to *Pro* is unfortunate, but it is a small price to pay for piece
  of mind and security.

* Mac ships with [FileVault](https://en.wikipedia.org/wiki/FileVault) disk
  encryption. However, it is not enabled by default, hence you must enable it
  in *Security & Privacy* settings.

* Linux distributions provide [LUKS](https://en.wikipedia.org/wiki/Linux_Unified_Key_Setup)
  disk encryption. Please enable full disk encryption at installation time.

* iOS platforms, iPhones and iPads, encrypt data by default. But the encryption
  will only be effective once a PIN has been enabled.

* Modern Android phones encrypt by default whilst most older Android phones do
  not. Old Android phones are insecure, hence it is recommended to upgrade to a
  modern Android device such as a Google Pixel phone. Please also set a strong
  PIN code.

* Modern NASs, such as QNAP and Synology devices, also provide encryption
  capabilities. If a NAS is used to store confidential information, such as
  financial details or family photos, then it is recommended to setup encrypted
  shares. Just be aware you will need to log into the management interface to
  unlock a share each time the NAS is started or rebooted; a very minor
  inconvenience.

Enable Firewalls
================

A computer [firewall](https://en.wikipedia.org/wiki/Firewall_(computing))
establishes a barrier between an inside network (such as your home network) and
an outside network (such as the internet). Firewalls are used to prevent
unauthorised access to a network like your home network.

* Modern versions of Windows ship with a firewall which will be enabled by
  default.

* Mac ships with a firewall, but does not enable it by default. Please enable
  the firewall in *Security & Privacy* settings.

* Linux Mint does not enable a firewall by default. Please install and enable
  the Uncomplicated Firewall (UFW):

  ```
  sudo apt install gufw
  sudo ufw enable
  ```

* Please also make sure that your router's firewall is enabled, sometimes it
  will be marked as *Stateful Packet Inspection* (SPI). Most modern routers
  will enable their firewall by default, many older routers do not. If you own
  an old router I recommend replacing it with a modern router.

Secure Your Router and Wi-Fi
============================

Weak router and Wi-Fi configuration is a common source of network insecurity,
it can allow hackers from the other side of the world, or wardrivers sitting
outside your house, into your home network.

Configuring a router can be daunting for those not experienced in technology.
Though modern routers are slightly less obtuse than they used to be. Again,
don't be afraid to call in help, better that than living with an insecure
router.

These simple rules will greatly increase the security of your router:

* Change the default administrator password of your router. This should be done
  **immediately**. As per-usual choose a very strong password. The default of
  `admin/admin` (username / password) is an
  [invitation to hackers](https://www.forbes.com/sites/leemathews/2017/09/13/equifax-website-secured-by-the-worst-username-and-password-possible/#238422e2457d)
  to compromise your network.

* When setting up Wi-Fi please activate encryption, that means
  configuring *WPA2 Personal* only, sometimes it will be marked as *WPA2-PSK*.

* Select a unique Wi-Fi identifier (SSID), do not use the router default
  identifier.

* Please choose an **extremely strong** Wi-Fi password, at least thirty
  characters long is desired.

* If your router provides guest Wi-Fi capability then please setup a separate
  secure guest network for all your smartphones. This segmented network
  architecture will protect your home network from any vulnerabilities
  contained in your older smartphones.

Install Reputable Software from Trusted Sources
=============================================

Be very cautious when installing software, apps or browser plugins. There is a
lot of malicious software on the internet posing as useful or legitimate
software. Only install reputable software and apps from official sources,
prefer installation from app stores such as the Apple App Store or Google Play
store.

Please **NEVER** install dubious software such as key generators, these more
often than not are malicious.

<a id="browser"></a>Browser Recommendations
===========================================

For all platforms, apart from iOS, I recommend the Google Chrome browser for
its enhanced security. From sandboxing, to hardened builds, to auto-updates, to
multi-platform support, Chrome is hard to beat. For iOS the best choice is the
default Safari browser.

Chrome security can be strengthened by the addition of these three excellent
extensions:

* [HTTPS Everywhere](https://www.eff.org/https-everywhere) is an extension that
  will force the use of connection encryption whenever possible. Certain web
  sites offer both plain-text and encrypted connections but will default to
  plain-text, these connections can be eavesdropped. This extension will force
  the secure encrypted connection in such circumstances.

* [uBlock Origin](https://chrome.google.com/webstore/detail/ublock-origin/cjpalhdlnbpafiamejdnhcphjbkeiagm?hl=en)
  is a content filtering extension most well known for blocking ads, but it
  also blocks content from sites known to host malware (malicious software).

* [Privacy Badger](https://chrome.google.com/webstore/detail/privacy-badger/pkehgijcmpdhfbdbbnkijodmdjhbjlgp)
  is an extension that learns to block trackers as you browse the web. This
  extension will increase the privacy and security of your browsing session.

<a id="windows"></a>Windows Anti-Virus
======================================

Back in the day running third-party Windows anti-virus was a necessity.
However, these days moderns versions of Windows ship with their own
[Defender](https://www.microsoft.com/en-us/windows/comprehensive-security)
product which is more than enough anti-virus for most people.

Instead of investing in an anti-virus product, I instead recommend using
[Malwarebytes](https://www.malwarebytes.com) anti-malware software. A free
version, for personal home use, is available that will need to be run manually
from time to time, whilst there is also a paid version that will offer real-time
protection.

[Windows Defender and
Malwarebytes](https://thewirecutter.com/blog/best-antivirus) compliment each
other and provide superior protection for modern versions of Windows.

Backups
=======

Backups are critical in case you experience either: a computer failure, a major
security incident that deletes your data, or a security incident that locks you
out of your data. [Ransomware](https://en.wikipedia.org/wiki/Ransomware) is an
example of such an attack, Ransomware will encrypt and lock your data
permanently unless you pay a ransom to a cybercriminal to unlock your data.
Note, paying a ransom will rarely gain you access to your data; please never
pay Ransomware.

Backups came in two major flavours, local or online. The former usually entails
USB drives, the latter will be internet based.

My preferred software for local backups:

* [Genie Timeline](https://www.genie9.com/home/Genie_Timeline_Home/overview.aspx)
  is user-friendly backup software for Windows users.

* [Time Machine](https://en.wikipedia.org/wiki/Time_Machine_(macOS)) is a
  simple and automatic backup solution provided by default with Apple Macs.

* [Deja Dup](https://launchpad.net/deja-dup) is a simple backup utility for
  Linux systems.

When using these local backup software **please** enable their encryption
support. You don't want your backups in plain text in case your lose your
backup drive or have it stolen.

If you already have a functional backup strategy, but you are not encrypting
your backups, then I strongly recommend you simply buy an
[Apricorn](https://www.apricorn.com/) drive and set a strong hardware PIN.

For online backup, that being backups that target the internet, I recommend the
[SpiderOak](https://spideroak.com/one) One service.

Please make sure that backups are running with regularity, at least once a
week.

Whichever approach you take, please have a backup strategy, any strategy will
do, since there will likely come a day where either your computer dies or you
are a victim of cyberattack. At that point you will be extremely grateful that
you had a backup to restore from. Backups are your digital insurance, just as
you insure your home and contents, so you should backup your data.

Multi-factor Authentication
===========================

[Multi-factor
authentication](https://en.wikipedia.org/wiki/Multi-factor_authentication)
(MFA) is a method of granting access only after a client has supplied multiple
valid pieces of identity evidence.

Most systems and services default to single-factor authentication, which more
often than not would be a solitary password or PIN.

Two-factor authentication (2FA) on the other hand requires two sources of
identification; for example, a password **and** a one time PIN code which may be
supplied by an authenticator app, or that may be SMS/emailed to you.

2FA is now available for many high profile services such as: [Google](https://www.google.com/landing/2step/),
[Facebook](https://www.facebook.com/help/148233965247823),
[Twitter](https://help.twitter.com/en/managing-your-account/two-factor-authentication)
and [Apple](https://support.apple.com/en-us/HT204915) to name a few.

2FA greatly increases account security and is **strongly** recommended for
[high value
targets](http://www.businessinsider.com/the-biggest-targets-for-hackers-2015-3).
However, most citizens are not high-value targets, so the question is whether
the inconvenience is worth the benefit? That is a judgement you need to make.
Currently, I recommend
[strong](https://bluz71.github.io/2017/11/12/sensible-computing-security-tips.html#passwords)
single-factor authentication for most persons.

Summary
=======

Just as it takes a little effort to secure your physical assets, it also takes
a little effort to secure your digital assets. But these days it really is
necessary, and it should not be too difficult if you follow these sensible
rules:

* Keep your devices and software up-to-date

* Use strong unique passwords everywhere

* Don't fall victim to Phishing, Social Engineering, or Malicious USB-stick scams

* Don't use public Wi-Fi, but if you must then only use it with a VPN

* Enable disk encryption where available

* Enable firewalls

* Secure your Router and Wi-Fi

* Only install reputable software from trusted sources

* Use the Chrome browser with HTTPS Everywhere, uBlock Origin and Privacy
  Badger

* Windows Defender and Malwarebytes are all the Windows anti-virus you need

* Enable two-factor authentication if you are a high value target

* Backup your data

Be secure and happy computing.

Related Articles
----------------

- [The Best Internet Security: Layers of Protection, and Good Habits](https://thewirecutter.com/blog/internet-security-layers)

- [Basic Computer Security: How to Protect Yourself from Viruses, Hackers, and Thieves](https://www.howtogeek.com/173478/10-important-computer-security-practices-you-should-follow)

- [Top 10 Secure Computing Tips](https://security.berkeley.edu/resources/best-practices-how-to-articles/top-10-secure-computing-tips)
