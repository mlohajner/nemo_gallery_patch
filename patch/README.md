
# Gallery Patch

## Nemo Icon View → Gallery View

**Minimal diff. Maximum effect.**

This patch makes Nemo's existing **Icon View** behave like a proper **Gallery View** when using large thumbnails.
- No new view mode.
- No new configuration.
- No complicated changes to Nemo.

Just make the existing thumbnail size actually work.

---

## Gallery View...

Nemo already has the pieces of puzzle, we just make a proper gallery out of those.  
It can show thumbnails.  
It can arrange them in a grid.  
It already has a thumbnail-size setting.  

The missing piece was making the layout properly follow large thumbnail sizes.

## Minimal diff

Only **two Nemo source files are changed**.

That's it:
- No new subsystem.
- No new view implementation.
- No giant refactor.

A very small change to the existing Icon View code is enough to turn large-thumbnail Icon View into a usable gallery.

---

## Tested

The patch has been tested and confirmed working with:

**Nemo 6.4.5**

---

## Apply

From the root of the Nemo source tree:

```bash
patch -p1 < nemo-thumbnail-gallery.patch
```

Then build Nemo normally.

---

Same Nemo.  
Same Icon View.  
Now with gallery capability

Gallery View, you say?  
...yes, thank you very much!
