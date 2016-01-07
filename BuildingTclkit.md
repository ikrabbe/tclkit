# Introduction #

Tclkit is now built using the kitgen package. This is a collection of makefiles and some C code that will make the tclkit executable and build the runtime virtual file system from the sources you use to build with. An earlier build script called 'genkit' was more difficult to maintain and used a pre-constructed virtual file system. All current maintenance is done on kitgen.

## Executable naming ##

Kitgen can build two vfs flavours of tclkit. The original flavour uses Metakit to read the vfs database. Metakit is written in C++ and on many systems this has proved problematic in distribution. One solution is to link the stdc++ library statically to the binary. Another is to use an alternative package `vlerq` for reading the database. Kitgen defaults to building vlerq-based executables.

The second issue is how or if Tk is included. Tk is can be linked statically to the executable, included in the vfs as a shared object or left out altogether. On Unix it is generally most useful to include Tk as a shared object in the vfs. This permits the executable to be used on systems without X11 libraries provided the scripts do not require the Tk package. On Windows however we need a gui-mode program and a command-line mode program as the system treats them differently. Therefore on Windows we usually link Tk statically and have a separate non-Tk executable.

On Windows the use of C++ in Metakit does not present any problem in distribution. However, if it is necessary to sign a starpack using an Authenticode signature then the vlerq-flavour of Tclkit must be used. This is because the presence of the Authenticode certificate on the starpack confuses metakit but vlerq has been fixed to handle this.

The naming conventions used are as follows:

  * tclkit - traditional tclkit includes metakit and incr-Tcl and Tk
  * tclkitsh - traditional metakit-flavour tclkit with incr-Tcl but **without** Tk.
  * tclkit-cli - vlerq-flavour, no Tk, no incr-Tcl.
  * tclkit-dyn - vlerq-flavour, Tk as shared object in vfs, no incr-Tcl
  * tclkit-gui - vlerq-flavoud, Tk linked to executable, no incr-Tcl.

## Prerequisites ##

To make a tclkit executable you will require a C compiler. gcc is used on all platforms and on Windows MSVC 6 can be used as well. To make metakit-based tclkits a C++ compiler is needed.

upx is useful to create smaller executables and may be used if present.

## Obtaining kitgen ##

kitgen is currently managed using <a href='http://git-scm.com/'>git</a>. Snapshots of the current version can be downloaded <a href='http://github.com/patthoyts/kitgen/archives/master'>from github</a> and the site will offer either zip or tar.gz archives for download. Alternatively the git repository can be cloned from using git from git://github.com/patthoyts/kitgen.git

## Configuring the build tree ##

Kitgen is used to build tclkit for Tcl 8.4, 8.5 and 8.6. Older versions of Tcl did not have support for virtual file systems and so cannot be used.

The kitgen tree now includes all the necessary extensions to Tcl required to make tclkits. You only need to supply a version of Tcl and Tk. The toplevel Makefile provides an example of how to configure the basic build tree for a given version.

The toplevel config.sh script is then used to create a makefile for the target system. For instance to create a makefile for an 8.5 tclkit for linux we might specify
> sh config.sh 8.5/linux-ix86 thread mk cli dyn

The options available are as follows:
<dl>
<dt>thread</dt>
<dd>If the <code>thread</code> parameter is passed to config.sh then the thread extension is added to the tclkit and <code>--enable-threads</code> is passed to all the projects. On unix systems the executable will be linked with pthreads or any other library appropriate for threaded builds of Tcl.</dd>

<dt>allenc</dt>
<dd>Include <b>all</b> Tcl provided encodings. By default only the most commonly used set of encoding files are included in the vfs. This excludes most Asian encodings (although japanese cp932 is included since 8.5.8).</dd>

<dt>allmsgs</dt>
<dd>Include <b>all</b> Tcl msgcat files. By default these are not included in the vfs.</dd>

<dt>tzdata</dt>
<dd>Include <b>all</b> Tcl timezone files. By default these are not included in the vfs.</dd>

<dt>mk</dt>
<dd>Configure to build metakit flavour executables in addition to the vlerq flavour binaries.</dd>

<dt>cli</dt>
<dd>Request building of the command-line tclkit-cli and tclkitsh if the <code>mk</code> options is also included.</dd>

<dt>dyn</dt>
<dd>Request for an executable with Tk included in the vfs as a shared object. This has no metakit-flavour version so only produces tclkit-dyn.</dd>

<dt>gui</dt>
<dd>Requests a full GUI mode tclkit with Tk statically linked into the binary. This is the standard mode for Windows builds. On Unix <code>dyn</code> is recommended instead. This creates tclkit-gui and tclkit if <code>mk</code> has been provided.</dd>

</dl>

## Building ##

Change into the target directory and run make.
> sh config.sh 8.5/linux-ix86 thread mk cli dyn
> cd 8.5/linux-ix86
> make

Check the build is ok using the validate.tcl script at the toplevel:
> ./tclkit-cli ../../validate.tcl

## Using MSVC ##
A NMAKE compatible makefile is included in kitgen. To build tclkit using msvc ensure that the build tree is configured as per above. Create a 8.5/win32-ix86 directory and create a Makefile in this directory. This file only needs to include the toplevel file and declare what type of builds are required. For instance:
```
all: lite heavy
!include ..\..\Makefile.vc
```
is sufficient to build both vlerq and metakit flavour tclkit binaries for Windows. In this directory just run `nmake`.