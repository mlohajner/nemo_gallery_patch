# Nemo desktop `Next slide`

A tiny bonus that goes along with `nemo_gallery_patch`: if you're browsing large wallpaper thumbnails in Gallery View, it's a short step to wanting a fast way to advance your desktop/wallpaper slideshow too.

Right-click the desktop → **Next slide** → the current wallpaper is replaced by the next one in Cinnamon's background slideshow.

Too small to be its own project, too useful to leave sitting in a shell history somewhere.

---

## What it actually does

This is **not** a custom script that scans a folder and re-implements a slideshow. It just calls Cinnamon's own slideshow logic and tells it to advance now instead of waiting for its timer.

Cinnamon ships a separate D-Bus service for this:

```text
/usr/share/cinnamon/cinnamon-slideshow/cinnamon-slideshow.py
```

It runs as `org.Cinnamon.Slideshow` on `/org/Cinnamon/Slideshow` whenever **Play backgrounds as a slideshow** is enabled in `Backgrounds` settings, and it already exposes a method for exactly this:

```xml
<interface name="org.Cinnamon.Slideshow">
  <method name="begin" />
  <method name="end" />
  <method name="getNextImage" />
</interface>
```

`next-image.nemo_action` just calls it:

```bash
gdbus call --session \
  --dest org.Cinnamon.Slideshow \
  --object-path /org/Cinnamon/Slideshow \
  --method org.Cinnamon.Slideshow.getNextImage
```

Same code path the internal timer uses - same playlist, same random/sequential order setting, no separate state to get out of sync.

---

## Install

```bash
mkdir -p ~/.local/share/nemo/actions
cp next-image.nemo_action ~/.local/share/nemo/actions/
nemo -q
```

`nemo -q` restarts the Nemo desktop process so the new action is picked up. Right-click an empty area of the desktop and **Next slide** should be in the menu.

---

## Requirements

* **Play backgrounds as a slideshow** must be enabled in `Backgrounds` settings. The D-Bus service only exists while the slideshow is running — with it off, `org.Cinnamon.Slideshow` has no owner on the session bus and the call fails with:

  ```text
  GDBus.Error:org.freedesktop.DBus.Error.ServiceUnknown
  ```

* `gdbus` (part of `glib2.0-bin` / `libglib2.0-bin`, already present on any Cinnamon install).

---
