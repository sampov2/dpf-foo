# Foo YC20

This is a Faust implementation of a 1969 designed Yamaha combo organ, the YC-20. In addition to the Faust code, it has a Gtkmm UI with Jack audio and midi support. A LV2 instrument plugin is planned but not yet executed.

Original YC-20 organs have a touch vibrato control, which is vibrato induced by horizontal movement of the keys. As there very few (almost none) midi keyboards which produce such information, this feature has been left out of the emulation. Instead of the touch vibrato control, the control panel hosts a "realism" switch.

The realism switch has four positions like the drawbars. Each position adds a "feature" to the previous position. Thus all three features are enabled at the last position. The positions are:

```
off 	unrealistic mode
2/4 	oscillator detune
3/4 	percussion manual bleed
4/4 	drawbar bleed
```

Also, unlike the real YC-20, this synthesizer provides additional separate outputs for the bass section and selected main voice. The volume of these outputs is controlled with the same controls as the main mixed output.

The balance and brightness controls are in fact continuous controllers even though the ui shows them as having four positions like the other switches.

The MIDI implementation does not care about midi channels. As Jack MIDI offers flexible routing, there is little need to filter messages based on midi channel. Please contact the author if this becomes a real problem though.

MIDI can be used to play the emulation, but also control the parameters. Here are the supported control changes:



| CC | control                                        |
|---:|------------------------------------------------|
|    |	**pots**									  |
| 50 |	Pitch										  |
|  7 |	Master volume								  |
| 51 |	Bass volume									  |
|    |	**vibrato section**							  |
| 12 |	Vibrato depth								  |
| 13 |	Vibrato speed								  |
|    |	**bass section**							  |
| 14 |	Bass 16' drawbar (notched)					  |
| 15 |	Bass 8' drawbar (notched)					  |
| 23 |	Bass manual switch ( value < 64 = off, otherwise on) |
|    |  **section I**								  |
|  2 |	Section I 16' drawbar (notched)				  |
|  3 |	Section I 8' drawbar (notched)				  |
|  4 |	Section I 4' drawbar (notched)				  |
|  5 |	Section I 2 2/3' drawbar (notched)			  |
|  6 |	Section I 2' drawbar (notched)				  |
|  8 |	Section I 1 3/5' drawbar (notched)			  |
|  9 |	Section I 1' drawbar (notched)				  |
|    |	**balance**									  |
| 16 |	Balance between sections I and II			  |
|    |	**section II**								  |
| 17 |	Section II brightness						  |
| 18 |	Section II 16' drawbar (notched)			  |
| 19 |	Section II 8' drawbar (notched)				  |
| 20 |	Section II 4' drawbar (notched)				  |
| 21 |	Section II 2' drawbar (notched)				  |
|    |	**percussion**								  |
| 22 |	Percussion drawbar (notched)				  |
|    |	**others**									  |
| 52 |	Realism switch								  |

*notched* controls can be set to 4 different positions

## ChangeLog

1.3.0 => 1.4.0 (??)
	- Change license from GPL to New BSD
	- Fix issue when changing samplerates (VST)
	- Add license screen and three more YC20 models
	- Integrate graphics into the binary

1.2.1 => 1.3.0 (January 28th 2011)
	- Graphical user interface for the GUI
	- OSX VST (Thanks Robin!)
	- Refactor UI codebase

1.2.0 => 1.2.1 (January 21st 2011)
	- Fix denormals in LV2 and VSTi

1.1.0 => 1.2.0 (January 20th 2011)
	- Windows and OS X releases
	- LV2
	- Improved volume control curve
	- Fixed bug with 4' drawbar bleed
	- More subtle hover highlights
	- Switch from gtkmm to straight gtk

1.0.0 => 1.1.0 (December 8th 2010)
	- Optimized band limiting scheme
	- Vectorized DSP code (built with faust -vec)

## Thanks

Comments, suggestions, whatever: contact the author:

 Sampo Savolainen <v2 'you know which symbol' iki.fi>

Thanks to a lot of people, especially:
 Robin Gareus,
 Petri Junno,
 Stéphane Letz

## Credits and licenses

Foo YC-20 uses the brilliant Cairo graphics library http://cairographics.org/
The library is licensed under LGPL 2.1 or MPL 1.1. The licenses are provided in
files LICENSE-cairographics-LGPL-2.1 and LICENSE-cairographics-MPL-1.1 .

The Windows VST plugin is statically linked against Cairo. As per the LGPL,
the object files required to link Foo YC20 against a different version of Cairo
are provided in the objects/ subdirectory in the binary distribution.

Here is the command to re-link the DLL using the MinGW compiler:

g++ -Wall -s -shared -mwindows -static objects/vsti.def objects/ringbuffer.o  objects/vsti.o objects/vstplugmain.o objects/foo-yc20.o objects/yc20-base-ui.o objects/win32.o objects/faust-dsp-plugin.o objects/graphics.o -o FooYC20.dll `pkg-config --libs cairo`

In addition to cairo, the statically linked VST DLL contains code from the
following projects: libpng, pixman and zlib. Thank you very much for all your
hard work!

VST is © 2004, Steinberg Media Technologies, All Rights Reserved

## Developer notes

Pulling all submodules: `git submodule update --init --recursive`

