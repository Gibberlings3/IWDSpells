IWDSPELLS CONVERTER

(crude readme)

Overview

This is an automated converter to pull spells from IWDEE to BG(2)EE, using leftover pieces of code from the converter that underlies IWDEE (and that descends from the old IWD-in-BG2 project). It works by starting with a spell file, and then iteratively going through every file that spell needs, every file those files need, and so forth. En route, it collects every relevant string from every language present in BG2. For wizard spells, scrolls are added, shadowing existing game scrolls (for instance, SHOUT scrolls occur everywhere CONE_OF_COLD scrolls do now).

Spells are added via ADD_SPELL; scrolls are slotted into empty spaces in the SCRL[0-9AZ][0-9A-Z] namespace. [edit: we changed over to using Cam's CDIA/CDID convention.] A few other files are renamed to avoid namespace conflicts; otherwise the IWD names are preserved. Files that are already present unmodified (or nearly so) don't get copied over.

It works as a standalone mod, but might be more useful included into something else. The virtues of automated conversion are (i) less change of errors creeping in; (ii) we can immediately and simply update to reflect any changes in IWDEE.

Spells implemented

See the files in data (iwd_arcane.2da, iwd_divine.2da, iwd_bard.2da) for the totally-reliable list. It's pretty much every IWD arcane and divine spell not already in BG2 (I don't do Contact Other Plane, for obvious reasons), plus (renamed) Mordenkainen's Sword, plus IWD versions of Conjure Elemental, plus all the bard songs.

Standalone Installation

1. Run the mod ("setup-dw_iwdspells") in your IWDEE root folder and install the one permitted component. (It doesn't matter what language you're using.) A folder, "dw_iwdspells_resource", will have been created.

Your IWDEE install should be completely unaffected, incidentally.

2. Move that folder into your BG(2)EE root folder.

3. Install the mod *again* in your BG(2)EE folder. There are 3 components: arcane spells, divine spells, bardsong.

Installation in another mod

1. Follow steps 1-2 above to create dw_iwdspells_resource

2. Put that folder somewhere in your mod. (It doesn't matter where, and you can rename it if you like.)

3. Put the folders "sfo", "ssl", "lib", "data", and "resource" somewhere in your mod. (Again, anywhere you like.)

4. Put the "dw_iwdspells_arcane.tra" and "dw_iwdspells_divine.tra" files, from tra/%LANGUAGE%, wherever your TRA files live. 

5. Put the file "dw_iwdspells.ini" somewhere in your mod.

6. Put this bit of code in your ALWAYS (or somewhere else it'll execute before the component is used), edited appropriately:

<beginning of code>
   // replace "dw_iwdspells" with your mod root directory
   OUTER_SPRINT scsroot dw_iwdspells 

   // replace "dw_iwdspells_resource" with the FULL path (relative to BG2EE) to this folder, e.g. "mymod/dw_iwdspells_resource" if you put it into your mod
   OUTER_SPRINT resource_loc dw_iwdspells_resource 

   // set all these to point to wherever you put the folders (relative to your mod!)
   OUTER_SPRINT iwdspells_data data
   OUTER_SPRINT iwdspells_lib lib
   OUTER_SPRINT sfo_loc sfo
   OUTER_SPRINT ssl_loc ssl
   OUTER_SPRINT iwdspells_resource resource

   // set this to point to wherever you put the file (ABSOLUTE LOCATION, e.g. mymod/dw_iwdspells.ini)
   OUTER_SPRINT inifile "%scsroot%.ini"

   // set this to point to wherever your TRA files live
   OUTER_SPRINT iwdspells_trabase tra

   // don't edit this
   INCLUDE ~%scsroot%/%iwdspells_lib%/always.tph~
<end of code>

7. Include the content of the three components in setup-dw_iwdspells.tp2 wherever you want them.

Translating the mod

Assuming your language is one of the languages in which IWDEE is installed, all you have to do is translate the dozen or so strings in the two tra files above (plus the three or four installation strings in setup.tra if you're using it standalone).

Modifications made to the spell system

While this is a fairly faithful implementation, there are a few places where thematic, conceptual or logical incompatibilities between the BG2 and IWD systems caused trouble. Sorting these out is a judgement call; I don't pretend these decisions are the only sensible choices.

1) Issue: The IWD and BG2 versions of Mordenkainen's Sword are completely different, but both quite cool

Fix: The IWD version is renamed "Mordenkainen's Force Blade" and given a spell.ids entry of WIZARD_MORDENKAINENS_SWORD_IWD

2) Issue: In BG2, there are "Conjure Lesser Elemental" spells at level 5 and "Conjure elemental" spells at level 6; in IWD, the L5 spells are just "Conjure Elemental". That creates thematic and namespace conflict, especially since BG2's summoning spells have the silly "elemental control" battle.

Fix: The IWD spells are renamed "Conjure lesser X Elemental", and are put into the IDS that way. The L6 Conjure Elemental spells have the elemental control battle removed. A level 6 version of Conjure Water Elemental is introduced. (Suboptimally, it shares a BAM with L5 Conjure Lesser Water Elemental.)

3) Issue: The IWD elemental animations artistically clash with the BG2 ones: solid humanoid figures vs much more slender and less humanlike ones.

Fix: Earth/Fire elementals get the BG2 animation. Water elementals get the water-weird animation (which in BG2 is already used for a water elemental in one place). 

4) Issue: IWD's Prayer spell (level 3) is about as powerful as BG2's Chant (level 2). IWD's Chant is much less powerful, giving about the same bonuses but halving movement and disabling spellcasting.

Fix: We split the difference, installing the IWD version of Chant over the BG2 one but removing the disable-spellcasting. (The half-movement remains.)

5) Issue: The IWD choices of summoned monsters clash with the various BG2 summons spells (for instance, the IWD versions can summon spiders or carrion crawlers, clashing a bit with Spider Spawn and Carrion Summons) and thematically are quite different from the BG2 versions of MS 1-3 (which summon humanoid creatures).

Fix: systematic restructuring of the summons to get something more thematically unified: MS spells pretty much exclusively summon humanoids. 

The Boneguard, previously summonable by MS VII, gets moved into its own Necromancy spell, "Create Boneguard". (Functionally it's MS VII, other than the school change and the fact that it's guaranteed to summon boneguards.) You can disable this in the ini file if it's a bridge too far.
[As of 9/17/2018 this is deprecated; I can do it in SCS instead.]

6) Issue: IWD restricts its Cause Wounds spells to evil casters, but the BG2 versions of Cause Critical/Serious Wounds aren't so restricted, so there's an inconsistency.

Fix: overwrite the BG2 versions of Cause Serious/Cause Critical Wounds with the IWD ones.

7) Issue: the BG2 Emotion:Hopelessness spell is a bit different from the other Emotion spells. 

Fix: overwrite the BG2 one with the IWD one.

8) Issue: IWDEE colors some icons green: it's not fully consistent as to which ones, but in particular all summoning spells get recolored. BG2 summoning spells are red, so there's a style clash.

Fix: most BG2 summoning spells have IWD versions; we overwrite their icons with the IWD ones. For the exceptions (e.g., Summon Planetar), we algorithmically recolor the icon from red to green. 
