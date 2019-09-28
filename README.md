# EDuke32 Fork for the AMC TC
This is a fork of the *EDuke32* engine that makes a number of changes specifically for the AMC TC.

**Link to the game: https://www.moddb.com/games/the-amc-tc**

It alters a number of hardcoded features in order to preserve backwards-compatibility, prevent game crashes and to improve the overall presentation of certain aspects of the game. These changes are most likely not suitable for a merge with the main eduke32 branch which is why we offer them here.

The changes that have been made to the engine are listed at the bottom of this document, and the specifics can be viewed in the corresponding SVN diff file. **We stress that this build is NOT INTENDED to be used with Duke Nukem 3D or ANY of its expansions.  Do not make attempts to do so, or else things will inevitably break.**

Instead, to play Duke Nukem 3D, please download an official *EDuke32* release from here: https://www.eduke32.com/ 

In this repository, we provide the source code for our fork of EDuke32, a patch file listing the changes that have been made to the respective revision this fork was based on, as well as the png images used for the startup banner and the game icons, which have been created by **AliCatGamer** and **Sebabdukeboss20**. We also provide pre-compiled executables for 64 and 32 bit Windows (compiled using gcc 9.2).

*EDuke32 is licensed under the GNU GPL 2.0 and the BUILD license, 
which have been included in the base directory of this repository as well, under gpl-2.0.txt and buildlic.txt.
We hereby release our changes to the engine under the same licenses.*

### This fork has been created by:
* Cedric "Sangluss" Haegeman
* Dino "Doom64hunter" Bollinger

### AMC TC Lead: 
* James Stanfield

### Art by:
* AliCatGamer
* Sebabdukeboss20 

**Huge thanks go out to the EDuke32 team for assisting us with feature requests, performance improvements and bugfixes over the many years of the game's development (and hopefully many more):**

### EDuke32 Credits:
* Developers:
   * Richard "TerminX" Gobeille
   * Evan "Hendricks266" Ramos
   * Alex "Pogokeen" Dawson
   * Pierre-Loup "Plagman"  Griffais
   * Philipp "Helixhorned" Kutin
* Special Thanks to:
   * Jonathon "Jonof" Fowler
* Uses Build Engine Technology by:
   * Ken "Awesoken" Silverman
* Additional Contributors:
   * Alexey Skrybykin
   * Bioman
   * Brandon Bergren
   * Charlie Honig
   * Dan Gaskill
   * David Koenig
   * Ed Coolidge
   * Emile Belanger
   * Fox
   * Hunter_Rus
   * James Bentler
   * Jasper Foreman
   * Javie Martinez
   * Jeff Hart
   * Jonathan Strander
   * Jordon Moss
   * Jose Del Castillo
   * Lachlan Mcdonald
   * LSDNinja
   * Marcus Herbert
   * Matthew Palmer
   * Matt Saettler
   * Nyoo123
   * Ozkan Sezer
   * Peter Green
   * Peter Veenstra
   * Robin Green
   * Ryan Gordon
   * Stephen Anthony
   * Tueidj

# Build Instructions:
Precompiled 64-bit Windows binaries of this fork are included directly with the download for the AMC TC 3.6, and can also be found in the ```windows-binaries``` subfolder of this repository.
 
If you are running on a different architecture/OS, or need to build the binaries yourself for other reasons, install the required dependencies on your system, and then run the makefile in the ./eduke32-amcfork directory.

**IMPORTANT NOTE: you will need to use the following parameter:**  ```make -AMCTC=1``` 

Afterwards, you can simply copy the resulting binaries to your AMC TC directory, and launch the game. 

For further information, see the instructions on the eduke32 wiki for the respective OS. 
   * Windows: https://wiki.eduke32.com/wiki/Building_EDuke32_on_Windows
   * Linux: https://wiki.eduke32.com/wiki/Building_EDuke32_on_Linux
   * MacOS (untested): https://wiki.eduke32.com/wiki/Building_EDuke32_on_macOS

This fork is currently based on eduke32 r8133. If you want to try a different version, 
you can use the diff file as a patch to a local copy of the official eduke32 SVN.
However, we do not recommend doing this, as changes between revisions can make the
game quite unstable.

### Mandatory Packages
* Basic dev environment (GCC >= 4.8, GNU make, etc)
* SDL2 >= 2.0 (SDL >= 1.2.10 also supported with SDL_TARGET=1)
* SDL2_mixer >= 2.0 (SDL_mixer >= 1.2.7 also supported with SDL_TARGET=1)
* libGL and libGLU (required for OpenGL renderers)
* libgtk+ >= 2.8.0 (required for the startup window)
* libvpx >= 0.9.0
* libvorbis >= 1.1.2
* libvorbisfile
* libogg         
### Optional Packages
* NASM (highly recommended for i686/32-bit compilation to speed up 8-bit classic software renderer)
* libFLAC >= 1.2.1

# Changelist
Here you will find a more or less comprehensive list of changes this fork makes to r8133 of the official eduke32 master branch.
For details, see the included diff file.

## Non-gameplay changes:
* Increased MAXTILES from 30720 to 32512 (which should hopefully suffice for Episode 4 and 5)
* Increased MAXSOUNDS from 4096 to 8192 (in both eduke32 and mapster32)
* Changed colors of walls with cstat 1 in mapster32 to blue. (editorcolors[9])
* Increased the default cachesize of eduke32 and mapster32 to 512 MB. This in particular prevents a startup crash for mapster32 with AMC.
* Updated the cache entry array so that it resizes dynamically (cache size in Bytes is still fixed however). This prevents crashes due to the huge number of tiles in both mapster32 and eduke32.
* Resized the startup windows for mapster32 and eduke32 to accomodate for the new banners (the banners are included in the "images" subfolder)
* Added (custom) indicators to hardcoded appnames to make it clear that this is NOT the regular eduke32 binary.
* Default appname for mapster32 testing now points to amctc.exe rather than eduke32.exe.
* To retain game progress from previous versions, the CFG is hardcoded to remain as `eduke32.cfg` even when compiled with different appname.
* Altered the position of the skill select menu to display skill info more cleanly (the latter being done through CON)
* per-map art is only reloaded into the cache if the map being loaded is a different one, preventing cache pollution on save load.
* Disabled Duke3D GRP autodetect (and all its various expansions and variations)
* Added AMC_BUILD compiler flag (used for changing certain hardcoded strings, and the menu layout)
* Added AMCTC gameflag (used for gameplay and menu alterations)

## Gameplay changes:
* Floor Z-height of the player is only clamped if he is not shrunk. This allows the player to pass under enemies again while in this state, which is currently broken in eduke32 as of this writing.
* Additional call to getzrange when player is shrunk to allow the player to get crushed by enemies when growing back.
* Increased the force by which the player is pushed downwards when shrunk. This is also a necessary component to make the player pass under enemies again for the crushing behavior. 
   * In addition, the above allows the player to pass through sprite-bridges when shrunk, a functionality which was utilized in one of our maps.
* Added a check: if player's yrepeat is 35, we assume he's performing a slide kick; and the "old" clipping behavior should kick in. 
   * this means that the z-velocity is added to z-position before clipmove() is called.
   * This re-enables being able to slide through tight gaps. To make sure that this does not affect gameplay elsewhere, we only enable this iff yrepeat = 35.
* Disabled hardcoded bloodsplats if SFLAG_BADGUY = 0. These are now implemented in scripts.
