---
layout: post
title: Fixing Broken Image Previews in Ranger on Urxvt
---

Die hard `Vim` users naturally gravitate towards programs that try to bring the
same keyboard-focused experience to other areas. You're probably here because
you use the(most excellent) `ranger` file manager on `rxvt-unicode`, but your
image previews are broken - there's flickering and thick black lines in them.
For a while I simply resorted to firing up an instance of `xterm` when I needed
to browse images, but recently fixed it as follows:

Get a `urxvt` with support for custom icons and backgrounds. You might have to
download a patched version.  For Arch Linux users, the `rxvt-unicode-pixbuf`
package in the AUR is what you need. Install it with `yaourt` as follows:

```
_$ yaourt -S rxvt-unicode-pixbuf
```

This conflicts with, and will replace your existing install.

Next, put the following in your `~/.config/ranger/rc.conf`:

```
set preview_images true
set preview_images_method urxvt
```

Optionally, you can have large previews that fill the entire terminal with the
`urxvt_full` method.

```
set preview_images_method urxvt_full
```

And we're done. Restart `urxvt`, browse to an image file, and revel in the
knowlegde that you have one less reason to leave your terminal.
