# Asset Overrides for Assassin's Creed Unity
A plugin for ACUFixes' Plugin Loader that enables runtime loading of AnvilToolkit mods to avoid repacking the large .forge files.

## Requirements
- Assassin's Creed Unity v1.5.0.
- `ACUFixes-PluginLoader.dll` v0.9.0

## How to install
Install the `ACUFixes-PluginLoader.dll` v0.9.0 or higher. `ACUFixes.dll` itself is not required.\
Place `AssetOverrides.dll` into `Assassin's Creed Unity/ACUFixes/plugins/`

## How to uninstall
Delete `AssetOverrides.dll` from `Assassin's Creed Unity/ACUFixes/plugins/`

## How to install asset mods
I'll use three different asset mods as an example:
- <a href="//www.nexusmods.com/assassinscreedunity/mods/136">Assassins Creed Victory Outfit v1.0 by LordOfThe9</a>
- <a href="//www.nexusmods.com/assassinscreedunity/mods/123">Sword of Altair by Halzoid98</a>
- <a href="//www.nexusmods.com/assassinscreedunity/mods/280">ACU-Alternate Walking animation by Petrichor23</a>

Each mod goes in a different folder.
1. Download the mods.
2. Unzip and arrange the files in folders as shown below.
```diff
Assassins's Creed Unity/
└── ACUFixes/
	└── plugins/
		└── AssetOverrides/
+			├── VictoryOutfit/
			│	└── Datapacks/   <<<--- All the .data files for your mod go here
			│		└── 1_-_CN_P_FR_LegacyAvatar_Altair.data
+			├── Alternate Walking Animation/
			│	└── LooseFiles/   <<<--- All the \"loose files\" (non .data files) go here
			│		├── 1_-_xx_Nav_Walk_3-Loop.Animation
			│		├── 1_-_xx_Nav_Walk_ShoulderTurnLt.Animation
			│		└── 1_-_xx_Nav_Walk_ShoulderTurnRt.Animation
+			└── Sword of Altair/
				├── Datapacks/   <<<--- All the .data files for your mod go here
				│	└── 0_-_GFX_SwordOfEden_Glow_DiffuseMap.data
				│	└── 1_-_CN_W_SwordOfEden_DiffuseMap.data
				│	└── 1_-_CN_W_SwordOfEden_NormalMap.data
				│	└── 1_-_CN_W_SwordOfEden_SpecularMap.data
				└── LooseFiles/   <<<--- All the \"loose files\" (non .data files) go here
					└── 1_-_SwordOfEden_LOD0.mesh

```
3. In game, open the ACUFixes ImGui menu, open the `AssetOverrides.dll` menu from the plugin list.
4. Make sure the "Enable Asset Overrides" and "Enable on startup" checkboxes are enabled.
5. Click "Refresh load order".
6. Select and right click to activate each asset mod.
7. Press "Save load order".
8. Press "Save config".
9. Restart the game.

That should be enough, you should now see the Altair Outfit, the Sword of Eden and Arno's walking animation replaced by the modded versions.

## Limitations
This is not a replacement for AnvilToolkit, merely a convenience feature
to avoid unpacking/repacking large .forge files in _some_ cases.

It is inconclusive for now whether or not the very act of swapping assets at runtime leads to _more_ crashes, but it doesn't seem to be the case. If you're experiencing crashes, you can install your mods the usual way via the AnvilToolkit which will always give you the most stable results.

If you're suspecting that some crashes are caused by this feature, then just delete the
```
Assassin's Creed Unity/ACUFixes/plugins/AssetOverrides.dll
```
to completely disable all Asset Overrides.

#### Cannot override objects until they stop being used

If the game object that you're trying to replace is _currently being used_
then it will not be overridden until it _stops being used_ and is unloaded first.
Going back to the VictoryOutfit example:
1. Start the game normally, don't enable any of the asset overrides.
2. Equip the Altair Outfit (the unmodded one).
3. Enable the VictoryOutfit in the load order.
4. The Altair Outfit remains unchanged. It will remain unchanged
   at least until you unequip it and resume the game.
   More precisely, it will remain unchanged until all usages/references
   of it are cleared and the datapack is unloaded.
   Only then will the modded outfit be able to load.

If you open the Load Order section and click on a mod, then on the right pane
you'll be able to see the detected datapacks within the mod, as well as their handles
	(`150860920202` in case of Altair Outfit and Victory Outfit,
	which is the handle of `CN_P_FR_LegacyAvatar_Altair\CN_P_FR_LegacyAvatar_Altair.EntityBuilder`) 
and the number of references.

When the number of references is green, it means that the datapack with this handle is currently in use.\
When the number of references reaches 0, the datapack will be unloaded, at which point the overrides can become active.\
Sometimes you'll need to restart the game so that a mod can be freshly loaded.\
This is the case for all _weapon_ replacers, for example.

#### Active in every World

Some AnvilToolkit mods are supposed to be installed in a particular World\
(repacked into `DataPC_ACU_LGS_BelleEpoque.forge`, `DataPC_ACU_Paris.forge`, `DataPC_ACU_Versailles.forge` etc.),\
but Asset Overrides are active in every World.\
This might be what you want, but can theoretically cause problems if mods are used
in a way they weren't supposed to be.
If you're experiencing problems, just disable Asset Overrides and try installing
your mod the usual way via the AnvilToolkit.

#### The game does not check for errors

The game does not check for file errors, so don't put junk files into your mod folders.\
In particular, the game can freeze or crash even at startup if a .data file is not a correct datapack.

## Supported mods
Examples of mods I've confirmed to work when installed through Asset Overrides:
- <a href="//www.nexusmods.com/assassinscreedunity/mods/133">Templar Extremist Retexture + Remove Distance Glow by MaceoniK</a>
- <a href="//www.nexusmods.com/assassinscreedunity/mods/39">Play As Elise by Halzoid98</a> (of the 3 .data files it's best to use only the one that goes in `DataPC_ACU_Paris.forge`. Also the face texture can be missing at first, but just switch to a different outfit and back - this seems to be a known issue.)
- <a href="//www.nexusmods.com/assassinscreedunity/mods/124">Elise in Lydia Frye's outfit by th3kill</a>
- <a href="//www.nexusmods.com/assassinscreedunity/mods/136">Assassins Creed Victory Outfit v1.0 by LordOfThe9</a>
- <a href="//www.nexusmods.com/assassinscreedunity/mods/271">Play as Ghost Face v1.00 by LeafyyIsHere</a>
- <a href="//www.nexusmods.com/assassinscreedunity/mods/255">Big Smoke by Kuza49</a>
- <a href="//www.nexusmods.com/assassinscreedunity/mods/228">ACU Original MOD Collection IV Female Character Outfit Port by nicholasllj</a>
- <a href="//www.nexusmods.com/assassinscreedunity/mods/197">ACU play as young Arno by Petrichor23</a>
- <a href="//www.nexusmods.com/assassinscreedunity/mods/198">Lucy Thorne Outfit by Kuza49</a>
- <a href="//www.nexusmods.com/assassinscreedunity/mods/222">Parkour Front flip and Jump mod by Petrichor23</a>
- <a href="//www.nexusmods.com/assassinscreedunity/mods/114">Connor's Tomahawk by Halzoid98</a>
- <a href="//www.nexusmods.com/assassinscreedunity/mods/147">AC Valhalla Basim Sword by IBSARP</a>
- <a href="//www.nexusmods.com/assassinscreedunity/mods/278">Excalibur Sword by Johntaber</a>
- <a href="//www.nexusmods.com/assassinscreedunity/mods/123">Sword of Altair by Halzoid98</a>
- <a href="//www.nexusmods.com/assassinscreedunity/mods/272">Classic Style Sync (Health) Regeneration by Petrichor23</a>
- <a href="//www.nexusmods.com/assassinscreedunity/mods/299">Less Frequent World Events (Paris) by knEke</a>
- <a href="//www.nexusmods.com/assassinscreedunity/mods/280">ACU-Alternate Walking animation by Petrichor23</a>


## Warning
Please make a back up of your savegame.
