A Tclkit is a single-file executable that provides a complete Tcl and Tk runtime and can execute both normal tcl scripts or script archive files known as StarKits. Tclkit is provided pre-built for a number of platforms and is updated to track Tcl releases for the major platforms (Windows, Linux and MacOS X).

# Downloading #

For pre-built files see the [TclkitDownloads](TclkitDownloads.md) page.

# Building from source #

See [BuildingTclkit](BuildingTclkit.md)

# License #

The Tclkit-specific sources are license free, they just have a copyright. Hold the author(s) harmless and any lawful use is permitted.

This does not apply to any of the sources of the other major Open Source Software components used in Tclkit, which each have very liberal BSD/MIT-like licenses:

> IncrTcl, Metakit, Tcl/Tk, TclVFS, Zlib

# Acknowledgements #

The original release of Tclkit relied heavily on Matt Newman's Tcl implementation of VFS, a "Virtual File System" layer for Tcl. Since March 2002, Tclkit uses the next generation C implementation by Vince Darley, which has become part of the standard Tcl core. Both their contributions and feedback are hereby very gratefully acknowledged.

Over the past several years, Steve Landers has contributed greatly, most notably by telling the story of Tclkit (and Starkits, and Metakit, and Critcl) through numerous presentations at Tcl and Unix conferences around the world.

Telindustries in the US, Steve Blinkhorn in the UK, and after them an increasing number of users of Tclkit, have helped take Tclkit forward by putting it to the real test and using this in production environments.
Tclkit ports

Many thanks also to a growing team of people who have contributed builds of Tclkit for various platforms - it wouldn't be in all those places without their help:

  * Reinhard Max
  * Daniel Steffen
  * Steve Landers
  * Tom Krehbiel
  * Donal Fellows
  * Bob Techentin
  * Detlef Groth
  * George Peter Staplin
  * David Zolli
  * Paul Obermeier
  * Pat Thoyts
  * ... with apologies if I forgot anyone!