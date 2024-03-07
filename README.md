DOOM-CRT
========

One thing I’ve wanted to do for some time, is take the original source 
code release for DOOM, the one that only had linux support, and make my 
own port to windows, utilizing my single header libs and putting my crt 
filter on top of it. This is that thing.

Now, there are a lot of great windows ports, fixing bugs and adding 
features, and most of them would be a better choice if you want a DOOM 
which runs well on windows, or if you want some code to do serious work 
on top of. My port won’t try to compete with those efforts.

The main reason I’m doing it is for my own enjoyment. I’ve never really 
poked around in the DOOM code properly before, but always wanted to.

And also, I think it might be useful to make available a simple and 
minimalistic port. One which modifies the original code as little as 
possible, uses as few dependencies as possible (no large external 
frameworks) and which can be built without a complex build system.

It is not using any large frameworks, but relies on a few stb-style
single-file header-only libraries (which are all placed in the `libs_win32`
folder) which I have written myself - with the notable exception of
the TinySoundFont library, which is used as a back-end for music playback.

It’s not a particularly nice port, quick and dirty is more like it. 
Feel free to fork it and clean it up if you want to :-)

I had to make a few extra changes to make it so you can build it to WASM
for running in a browser, but those changes are contained to a single commit,
so if you don't want them, just checkout the commit prior to that one.

How to build
------------
To build, start a visual studio developer command prompt and run the command:

    cl doom.c

This should give you a runnable `doom.exe`. The wad file for the 
shareware version is in the repo as well, so you should be good to run
the exe straight away. Enjoy!

You can also build with Tiny C Compiler:
	
	tcc\tcc doom.c
	
You can download tcc here: [tiny-c-compiler](https://github.com/mattiasgustavsson/template_project/releases/tag/tiny-c-compiler) for either 32 or 64 bit Windows. Unzip it so that the `tcc` folder in the zip file is at your repository root.

It can also be built into an HTML file to run in a browser, using WAjic:

	wasm\node wasm\wajicup.js doom.c doom.html -embed doom1.wad doom1.wad -rle -template template.html

A WebAssembly build environment is required. You can download it (for Windows) here: [wasm-build-tools-win](https://github.com/mattiasgustavsson/template_project/releases/download/wasm-build-tools/wasm-build-tools-win.zip).
Unzip it so that the `wasm` folder in the zip file is at your repository root.

The wasm build environment is a compact distribution of [node](https://nodejs.org/en/download/), [clang/wasm-ld](https://releases.llvm.org/download.html),
[WAjic](https://github.com/schellingb/wajic) and [wasm system libraries](https://github.com/emscripten-core/emscripten/tree/main/system).


/Mattias Gustavsson

------------------------------------------------------------------------
## ORIGINAL README FROM ORIGINAL SOURCE CODE RELEASE FOLLOWS
------------------------------------------------------------------------

Here it is, at long last.  The DOOM source code is released for your
non-profit use.  You still need real DOOM data to work with this code.
If you don't actually own a real copy of one of the DOOMs, you should
still be able to find them at software stores.

Many thanks to Bernd Kreimeier for taking the time to clean up the
project and make sure that it actually works.  Projects tends to rot if
you leave it alone for a few years, and it takes effort for someone to
deal with it again.

The bad news:  this code only compiles and runs on linux.  We couldn't
release the dos code because of a copyrighted sound library we used
(wow, was that a mistake -- I write my own sound code now), and I
honestly don't even know what happened to the port that microsoft did
to windows.

Still, the code is quite portable, and it should be straightforward to
bring it up on just about any platform.

I wrote this code a long, long time ago, and there are plenty of things
that seem downright silly in retrospect (using polar coordinates for
clipping comes to mind), but overall it should still be a usefull base
to experiment and build on.

The basic rendering concept -- horizontal and vertical lines of constant
Z with fixed light shading per band was dead-on, but the implementation
could be improved dramatically from the original code if it were
revisited.  The way the rendering proceded from walls to floors to
sprites could be collapsed into a single front-to-back walk of the bsp
tree to collect information, then draw all the contents of a subsector
on the way back up the tree.  It requires treating floors and ceilings
as polygons, rather than just the gaps between walls, and it requires
clipping sprite billboards into subsector fragments, but it would be
The Right Thing.

The movement and line of sight checking against the lines is one of the
bigger misses that I look back on.  It is messy code that had some
failure cases, and there was a vastly simpler (and faster) solution
sitting in front of my face.  I used the BSP tree for rendering things,
but I didn't realize at the time that it could also be used for
environment testing.  Replacing the line of sight test with a bsp line
clip would be pretty easy.  Sweeping volumes for movement gets a bit
tougher, and touches on many of the challenges faced in quake / quake2
with edge bevels on polyhedrons.

Some project ideas:

Port it to your favorite operating system.

Add some rendering features -- transparency, look up / down, slopes,
etc.

Add some game features -- weapons, jumping, ducking, flying, etc.

Create a packet server based internet game.

Create a client / server based internet game.

Do a 3D accelerated version.  On modern hardware (fast pentium + 3DFX)
you probably wouldn't even need to be clever -- you could just draw the
entire level and get reasonable speed.  With a touch of effort, it should
easily lock at 60 fps (well, there are some issues with DOOM's 35 hz
timebase...).  The biggest issues would probably be the non-power of two
texture sizes and the walls composed of multiple textures.


I don't have a real good guess at how many people are going to be
playing with this, but if significant projects are undertaken, it would
be cool to see a level of community cooperation.  I know that most early
projects are going to be rough hacks done in isolation, but I would be
very pleased to see a coordinated 'net release of an improved, backwards
compatable version of DOOM on multiple platforms next year.

Have fun.

John Carmack
12-23-97

Copyright (c) ZeniMax Media Inc.
Licensed under the GNU General Public License 2.0.
