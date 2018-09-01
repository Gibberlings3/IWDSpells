
Modifications made to the spell system

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

6) Issue: IWD restricts its Cause Wounds spells to evil casters, but the BG2 versions of Cause Critical/Serious Wounds aren't so restricted, so there's an inconsistency.

Fix: overwrite the BG2 versions of Cause Serious/Cause Critical Wounds with the IWD ones.

7) Issue: the BG2 Emotion:Hopelessness spell is a bit different from the other Emotion spells. 

Fix: overwrite the BG2 one with the IWD one.

8) Issue: IWDEE colors some icons green: it's not fully consistent as to which ones, but in particular all summoning spells get recolored. BG2 summoning spells are red, so there's a style clash.

Fix: most BG2 summoning spells have IWD versions; we overwrite their icons with the IWD ones. For the exceptions (e.g., Summon Planetar), we algorithmically recolor the icon from red to green. 
