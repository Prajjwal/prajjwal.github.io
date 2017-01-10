---
layout: post
title: A List of Privacy Measures
---

... that you can take without being labeled 'crackpot'.

This aims to document everything I use to maintain a degree of privacy in **my**
digital life, along with a few comments. It is targeted at intermediate Linux
users who can get everything setup without any hand holding. I had wanted to
write tutorials on what follows, but that would make the post unbearably long.
Instead, I shall try to link to pages that are good starting points.

I intend to constantly update this, so it might be a good page to bookmark.

## Goals

- Achieve as much privacy as possible, without sacrificing(too much)
  convenience. The threshold varies from person to person. Personally, I'm not
  going to give up on GMail and do something crazy like run my own private email
  server, but I do bother encrypting my chats. The balance I have struck may
  seem excessive to some, and most deficient to others. Use this document as a
  reference to find your sweet spot.
- Understand that privacy / [security is not
binary](https://systemoverlord.com/2014/09/05/security-not-a-binary-state/).
A lot of people who dismiss efforts to make your digital life more private view
it that way. The point is *not* to be completely immune to the NSA, your
friendly neighborhood ad company, or whoever else is spying on you.  Your CPU
[has complete control over your PC](https://libreboot.org/faq/#intelme), and
maybe the [NSA can factor a certain prime that allows them to decrypt a large
portion of encrypted internet
traffic](https://freedom-to-tinker.com/2015/10/14/how-is-nsa-breaking-so-much-crypto/).
We're way past the point of being able to completely secure ourselves. The point
is to:
  - Make it harder for them to spy on you.
  - Limit the number of entities spying on you at any given point.

---

## Desktop

### Operating System

The obvious choice is Linux. Here's a list of distributions you should try out
if you don't already use it, in decreasing order of n00b friendliness. I
personally use Arch Linux.

- [Linux Mint](http://linuxmint.com/)
- [Elementary OS](http://elementary.io/)
- [Ubuntu](http://ubuntu.com/)
- [Arch Linux](https://www.archlinux.org/)
- Reddit user
[boarhog](https://www.reddit.com/r/linux/comments/5n6o0z/a_list_of_privacy_measures_you_can_take_without/dc97hrt/)
likes [Fedora](https://getfedora.org/)

You could also choose a flavor of BSD, and most of what follows would apply to
you.

### Firejail

Most applications on your system often have access to your entire file system.
That includes `~/.ssh`. Let that sink in for a minute. Proprietary code that you
run on your system could be uploading your ssh keys, your browser profile, and
your unencrypted chat history to who knows where. There is also precedent for
the free and open source Firefox being [exploited to steal sensitive
data](http://arstechnica.com/security/2015/08/0-day-attack-on-firefox-users-stole-password-and-key-data-patch-now/).

To mitigate this, I lock applications down with
[Firejail](https://firejail.wordpress.com/).

> Firejail is a SUID program that reduces the risk of security breaches by
> restricting the running environment of untrusted applications using Linux
> namespaces and `seccomp-bpf`. It allows a process and all its descendants to
> have their own private view of the globally shared kernel resources, such as
> the network stack, process table, mount table.

What he said.

**Here's what I've got sandboxed on my PC:**

- Firefox
- [Dropbox](http://dropbox.com/). This doesn't need to access anything but
`~/Dropbox`, and `~/.dropbox-dist`. There's some compulsive update behavior,
  where it repeatedly downloads an update, but is unable to actually update
  itself in this profile. I haven't figured out a solution to it yet.
- [Gajim](https://gajim.org/) - my primary XMPP client.
- Chromium
- [SpiderOakONE](http://spideroak.com/) - a backup program.
- qBittorrent
- LibreOffice

### qBittorrent

- Enable [Anonymous
Mode](https://github.com/qbittorrent/qBittorrent/wiki/Anonymous-Mode).
- Set a strong password for the Web UI, if enabled.

### Firefox

The declining market share of this browser compels me to include a 'Why Use
Firefox' section before we go any further.

- Performance is good enough, on both Deskop & Android. Those of you who were
driven into the comforting, yet evil embrace of Google because Firefox felt
slow, do give it another try now with
[Electrolysis](https://wiki.mozilla.org/Electrolysis) enabled. Feels like
butter.
- Mozilla is slowly replacing the rendering engine with
[Servo](https://github.com/servo/servo) - a lightning fast engine that leverages
your GPU for performance.
- Because Mozilla is committed to the open web.
- Because later, you [might not get a
choice](http://robert.ocallahan.org/2014/08/choose-firefox-now-or-later-you-wont.html).

**Use the following addons:**

- [uBlock
Origin](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/): Lean ad
blocker that does its job without making your browser [dog slow](), or [selling
your browsing history]().
- [Self Destructing
Cookies](https://addons.mozilla.org/en-US/firefox/addon/self-destructing-cookies/):
Removes cookies once you close a particular tab, thus greatly reducing the
number of ad cookies plotting murder on your system.
- [Decentraleyes](https://addons.mozilla.org/en-US/firefox/addon/decentraleyes/)
- [Remove Cookies for
Site](https://addons.mozilla.org/en-US/firefox/addon/remove-cookies-for-site/):
For when you want to take cookie murdering into your own hands.
- [uMatrix](https://addons.mozilla.org/en-US/firefox/addon/umatrix/)

Also, follow [this excellent guide](https://www.privacytools.io/#about_config)
to tweaking Firefox settings for maximum privacy. I don't personally have all of
this disabled, notably WebGL.


## File Sharing / Backup

### EncFs

[EncFs](http://www.arg0.net/encfs) transparently encrypts a folder on your
system. You get a folder with encrypted data that you can back up on Dropbox,
  which you can mount over FUSE and access files as you would normally.

The killer feature you should look at is reverse mounting, ie, EncFs can mount a
regular unencrypted directory on your system as an encrypted mount, which you
can subsequently backup using your favorite backup program.

Consider using `AES-CBC` mode, and also obfuscate file names.

### Dropbox

I don't leave it running 24x7, but manually do so when I need to sync something.
It's heavily sandboxed using `Firejail`.

### SpiderOakONE

My one and only gripe with this program is that it isn't open source, which
negates every claim of "zero knowledge" and "privacy" that they've made since
its conception. Fortunately, the three directories that I do need constantly
backed up in the cloud are actually `EncFs` mounts. I've got a cron job to run
`SpiderOakONE --batchmode` every three hours.

### file.io

[file.io](http://file.io/) basically deletes your file after it is downloaded.
I've got a small [shell script that uploads to
file.io](https://github.com/Prajjwal/dotfiles/blob/master/bin/fileio) which I
use all the time. Consider encrypting manually with openssl before you upload
here.

### Also Check Out

- [RClone](rclone.org). This is good for two things:
  - Keeping a directory in sync with cloud services that do not have FOSS
  clients, such as Dropbox.
  - Encrypting that sync.
- [Borg Backup](https://github.com/borgbackup/borg) - deduplicating backup that
also supports encryption.

## Android

Here's the thing about Android - if you really care about privacy, don't run it.
It's probably logging everything from your keystrokes to contacts. If you aren't
that hardcore, then there are steps you can take to limit the amount of data
Google gets.

- Use [Firefox for
Android](https://play.google.com/store/apps/details?id=org.mozilla.firefox) +
[uBlock Origin](https://addons.mozilla.org/EN-US/android/addon/ublock-origin/).
Performs as well as Chrome(if not better) on a [budget
phone](http://www.gsmarena.com/xiaomi_redmi_2_prime-7480.php).
- Limit the number of applications you install, prefer using their mobile web
app. Using [m.facebook.com](https://m.facebook.com) in your browser is much
better than using their [security nightmare of an
app](http://www.independent.co.uk/life-style/gadgets-and-tech/facebook-can-work-out-what-youre-watching-by-listening-through-your-smartphone-9444353.html).
Firefox also allows you to pin certain pages to your home screen, so you can
launch them as you would an app.
- Consider using a third party keyboard app, such as [SwiftKey](), and
completely block its access to the internet.
- Carefully go through app permissions on your device, and block anything that
the app doesn't need. Most apps don't need to access your contacts, read your
messages, or have full internet access.
- Turn off *Share usage statistics* and *share snippets* options in GBoard.
- Consider using [F-Droid](http://f-droid.org/) instead of Google Play.

## IM

Most of my conversation happens with a tiny group(< 5) of friends. I've
therefore moved them to public XMPP servers, and we now use open source clients
with end to end encryption to chat. Outside this group, I use whatever the other
person is using. I might do a future post detailing my setup.

### Clients

- [Gajim](http://gajim.org/) on the desktop.
- [Conversations](http://conversations.im/) on the phone.

### Encryption

I use the [OMEMO](https://en.wikipedia.org/wiki/OMEMO) protocol, that supports
group chats, file transfers, and offline messaging. If you're still on
[OTR](https://otr.cypherpunks.ca/), you need to upgrade.

- Here's the [Gajim OMEMO Plugin]().
- Conversations supports it out of the box.
- I've heard the [Swift XMPP Client](http://swift.im/) plans on supporting it in
the near future.
- [This](https://chatsecure.org/blog/chatsecure-conversations-zom/) may be your
best bet if you want OMEMO on iOS.

### Other Apps

If you can't coax your friends to run XMPP, try getting them on one of the
following apps.

- [Signal](https://play.google.com/store/apps/details?id=org.thoughtcrime.securesms)
- [Threema](https://threema.ch/en/)
- [Wire](https://wire.com/)

## Email

I'm dependent on GMail's web UI + keyboard shortcuts too much to move away from
it. Maybe someday.

## Miscellaneous

- Use [DuckDuckGo](https://duckduckgo.com/) as your search engine. You need to
be more specific with your searches, but it's worth it. Their [bang
syntax](https://duckduckgo.com/bang) will save you a lot of time.
- Consider enabling [Do Not Track](https://en.wikipedia.org/wiki/Do_Not_Track).
- Use [OpenNIC DNS servers](https://en.wikipedia.org/wiki/OpenNIC) instead of
Google DNS.
- Purchase WHOIS protection for your domain names(thanks
    [rowty1](https://www.reddit.com/user/rowty1).
- Use [KeePass](http://keepass.info/) for password management.

---

## Reading & Important Links

- [Why We
Encrypt](https://www.schneier.com/blog/archives/2015/06/why_we_encrypt.html)
- [I have nothing to
hide](https://www.wired.com/2013/06/why-i-have-nothing-to-hide-is-the-wrong-way-to-think-about-surveillance/)
- [Security: Not a Binary
State](https://systemoverlord.com/2014/09/05/security-not-a-binary-state/)
- [EFF Secure Messaging Score Card](https://www.eff.org/node/82654)
- [PrivacyTools](https://www.privacytools.io/)
- [Prism Break](https://prism-break.org/en/)

---

## Contributing

Since this outlines **my** personal privacy setup, I won't be accepting any
direct suggestions. If, however, I end up using something you showed me, I'll
put it in here. I'll give credit where credit is due, of course.

Hit up [@prajjwalsin](http://prajjwal.com/@) on Twitter for any feedback.
