[![CC BY-NC-SA 4.0][cc-by-nc-sa-shield]][cc-by-nc-sa]

# LT_CustomMapExample
A Unity project to facilitate the creation of custom maps for Gorilla Tag. 

**Contents**

* [Setup](#setup)
* [Creating a map](#creating-a-map)
* [Accessing Your Map](#accessing-your-map)
* [Trigger Scripts](#trigger-scriptsprefabs)
    + [Map Boundary](#map-boundary)
    + [Teleporter](#teleporter)
    + [Tag Zone](#tag-zone)
* [Placeholder Script and Prefabs](#placeholder-script-and-prefabs)
    + [Water Volume](#water-volume)
* [Other Scripts](#other-scriptsprefabs)
    + [Surface Override Settings](#surface-override-settings)
    + [Access Door Placeholder](#access-door-placeholder)
    + [Destroy Pre Export](#destroy-pre-export)
    + [Teleport Point (Prefab)](#teleport-point-prefab)
* [Exporting and Uploading to Mod.io](#exporting-and-uploading-to-modio)
    + [Exporting](#exporting)
    + [Uploading to Mod.io](#uploading-to-modio)
* [Playing Your Map](#playing-your-map)

## Setup
This project is made with Unity version 2022.3.2f1. Higher or lower Unity versions may not work properly, so make sure 
to download it from the [Unity Archive](https://unity3d.com/get-unity/download/archive) if you don't have it already. 
It's recommended to use Unity Hub to make managing versions easier.

**MAKE SURE TO ADD ANDROID BUILD SUPPORT TO YOUR UNITY 2022.3.2f1 INSTALLATION!** This is needed to make sure your 
bundles properly support the Quest. Instructions can be found here: 
https://docs.unity3d.com/Manual/android-sdksetup.html

## Creating a map
For the most part, creating a map itself is the same as creating anything in Unity. However, there are a few specific 
things that you'll need to do to ensure that Gorilla Tag can load it correctly.

To load up the Unity project, go ahead and open up Unity or Unity Hub (Hub is recommended) and then click Open/Add and 
navigate to the downloaded + unzipped project. Navigate to the folder that contains the `Assets`, `Packages`, and 
`ProjectSettings` folders, then click `Select Folder`.

If Unity doesn't load the Example Map upon opening you can open it by navigating to `Assets/Scenes/ExampleMap.unity`.
You can use this map as a reference for all the available functionality and how to properly setup a Map.

When creating a new map, you should first create a new Scene and load it up. Then create an empty GameObject that will 
hold everything in your map. Make sure the position is (0, 0, 0), and the scale is (1, 1, 1).

Next, click Add Component and add a Map Descriptor. This will hold some information about your map.

![mapdescriptor](https://github.com/user-attachments/assets/78bed4ad-5042-4a26-a2b8-ac094ad3664c)

Here's what each setting does:
- `Map name` - This will be used for your exported scene and the .zip file created by the export process.
- `Custom Skybox` - A cubemap that will be used as the skybox on your map. If this empty, it'll automatically give your

## Accessing Your Map
In order for Players to be able to access your map, it's required to include an `AccessDoorPlaceholder`.  

Under MapPrefabs, there's an `AccessDoorPlaceholder` prefab. Drag it into your scene and place it wherever you'd like. 
This represents the `Lobby` room that Players will use to pick which map to load. The door should be able to open 
directly into the map. Ensure the Placeholder doesn't overlap with any other meshes or collision or it could cause issues
for Players trying to load into your map. 

Players should always be able to return to the "Lobby" room at any point to facilitate leaving your map. Don't put it 
somewhere difficult to reach.

## Matching Gorilla Tag's Style
Although not every custom map has to look exactly like the game, making your map look similar to the base game's visuals 
will help improve player experience, so here's a couple of tips:

To make your textures have the same low-poly PS2 style as Gorilla Tag, change the following settings:
- Filter Mode - Point (no filter) [This will ensure your textures aren't blurry]
- Max Size - 64/128/256 [This will depend on your texture's size and what you're using it on]
- Compression - None [This will make sure that your images don't get garbled by compression]

![default_best_textureSettings](https://github.com/user-attachments/assets/920305a3-efcd-43f2-88d2-e26f02addca6)

Additionally, if you want to make a model low poly you can add a Decimate modifier to it in Blender. Lower the threshold 
until the model looks low poly enough for you.

![decimate_modifier](https://github.com/user-attachments/assets/f66907c0-132a-431b-8139-0ea3c7e7c9d1)

If some materials look washed out ingame, try changing these settings on those materials:
- Set Metallic to 0
- Set Smoothness to 0
- Turn off Specular Highlights
- Turn off Reflections

## Trigger Scripts/Prefabs
The following scripts can be used for multiple purposes, but the GameObject they are attached to will always need a Collider 
component with `Is Trigger` set to true. All trigger scripts have several common options: 
- `Triggered By Hands` - Should the player's hands activate this trigger?
- `Triggered By Body` - Should the player's body activate this trigger?
- `Triggered By Head` - Should the player's head activate this trigger?
- `Retrigger After Duration` - Should this trigger re-activate after a delay?
- `Retrigger Delay` - If `Retrigger After Duration` is set to true, what is that duration in seconds?

### Map Boundary
This trigger script will teleport the player to a random (or specific) Transform and optionally can Tag the player when 
triggered. It can be used in multiple ways, but the main intent was for it to be used as a way to prevent players from 
escaping your map. This script is included in the `MapBoundary` prefab in the `Assets/MapPrefabs/` folder which uses a 
Box Collider and includes a visual preview.

**Additonal Options**
- `Teleport Points` - One or more points the player will be teleported to when activating this trigger. If more than one
  point is defined, it will be chosen at random.
- `Should Tag Player` - Should the player be tagged when activating this trigger?
 
### Teleporter
This trigger script will teleport the player to a random (or specific) Transform. This script is included in the `Teleporter` 
prefab in the `Assets/MapPrefabs/` folder which uses a Box Collider and includes a visual preview.

**Additonal Options**
- `Teleport Points` - One or more points the player will be teleported to when activating this trigger. If more than one
  point is defined, it will be chosen at random.

### Tag Zone
This trigger script will Tag any activating player. This script is included in the `Tag Zone` prefab in the `Assets/MapPrefabs/` 
folder which uses a Box Collider and includes a visual preview.

## Placeholder Script and Prefabs
The `Placeholder` script defines an object that will get replaced by an existing Gorilla Tag script/object when your map is loaded 
in-game. Each has a prefab in the `Assets/MapPrefabs/` folder that can or should be used depending on the placeholder selected for 
the `Placeholder Object` setting.

### Water Volume
The `WaterVolumePlaceholder` prefab has the `Placeholder` script setup to use the `Water Volume` option with some default settings. It includes a visual preview and can be scaled as desired.

## Other Scripts/Prefabs

### Surface Override Settings
If you want to modify how climbing works on an object, you can add a `Surface Override Settings` script to it.

**Script Options:**
- `Sound Override` - Used to customize what sound plays when a Player hits the object.
- `Extra Vel Multiplier` - A number that influences how much extra velocity is gained when a player jumps off the object.
  (Must be higher than 1)
- `Extra Vel Max Multiplier` - A number that defines the maximum extra velocity multiplier applied when a player jumps off the
  object. (Must be higher than 1)
- `Slide Percentage` - A number that decides how "slippery" an object is when used for climbing. Default value is 0.0 which is
  the least slippery an object can be. Higher values are more slippery with a maximum of 1.0 meaning the object is unclimbable.

### Access Door Placeholder
This is used for positioning your map in the correct place so it lines up with the "Lobby" room in GorillaTag. This script
is already part of the `AccessDoorPlaceholder` prefab in MapPrefabs, so it's not necessary to manually add this to anything.
There should only be one `AccessDoorPlaceholder` script component in your map, if you place multiple, only one will be valid.

### Destroy Pre Export
This is used to destroy in-editor visualization helpers and other editor-only objects to ensure they don't end up in your 
exported map. You can attach this script to any GameObject that should **NOT** be included in your exported map.

### Teleport Point (Prefab)
This is a simple prefab that is essentially just a visual preview to show where you've placed them in your map. Can be used with the 
`MapBoundary` and `Teleporter` scripts to define teleport destinations.


## Exporting and Uploading to Mod.io

### Exporting

Once your map is all done, it's time to export! First, let's run through our checklist:

- Did you add Colliders to Objects that the player needs to collide with?
    - Make sure your colliders match fairly closely to the mesh shape.
    - Avoid using too many Mesh Colliders if possible, instead use Box or Sphere colliders
  (if you know what you're doing, you can always create lower poly versions of your meshes to use for the colliders)
- Did you completely fill out your `Map Descriptor`?
- Did you add the `AccessDoorPlaceholder` prefab to your map?
    - It's required to have an `AccessDoorPlaceholder` in your map, or you won't be able to export.
    - Brush back over the tips in the [Accessing Your Map section](#accessing-your-map) if needed
- Did you read over the [Lighting section](#lighting) and follow all the steps?

If you want to use a custom skybox, import it into your Unity project as an image, set the `Texture Shape` to Cube 
and assign it to the `Custom Skybox` property on your `Map Descriptor`

Now that you've gone over the checklist, it's time to export! Select the GameObject with your `MapDescriptor` component,
and click the `Export Map` button. 

This opens up to the `Exports` folder, but you can select any folder to export to. Click save, and once the export is 
finished you'll have a `.zip` file that's ready to upload to [Mod.io](https://mod.io/g)

### Uploading to Mod.io

After you've exported your map and have a `.zip` file ready, you can now upload your map to [Mod.io](https://mod.io/g). 
Go to [https://mod.io/g/gorilla-tag](https://mod.io/g/gorilla-tag) and create an account if you haven't already. 
Once you're logged in, click the `Add Level` button on the top right. 

![addLevelbutton](https://github.com/user-attachments/assets/b3d10471-e843-4442-8acc-5b85d51446d4)


On the following page you'll need to fill out some info about your map. 
The required fields are:
- Name - *try to make this unique to your map*
- Summary - *a brief summary of your map (minimum of 10 characters)*
- Logo - *this is the main image shown when players are browsing available maps*

The other fields are all optional, but be sure to fill out any information you'd like to. 

![requiredfields](https://github.com/user-attachments/assets/1d663db1-2bdf-4878-8c6b-4193a7ebc356)

Once your done filling out basic information, click the `Create Level` button at the bottom of the page.
On the next page are more optional fields. You can upload more screenshots of your map, link to a Youtube channel, or add links to
Sketchfab models.
Once you're done on that page click the `Save & next` button at the bottom of the page.

![media](https://github.com/user-attachments/assets/310fb041-84d3-4272-a4eb-c667ed519c85)

The next page is where you will upload your `.zip` file you exported from Unity. Click the `Select zip file` button and find the 
`.zip` file you exported. You can also add a Version number and Changelog with this file and each additionl file you upload. Once 
you've selected your `.zip` file and filled out any desired fields, make sure you read and agree to the mod.io Terms and Conditions 
and check the `I agree` box, then click the `Upload & next` button to continue.

![selectfile](https://github.com/user-attachments/assets/ce2405e7-830d-4631-bf97-c7a9e84a19f7)


The next page is for selecting any dependencies. This is currently unsupported for Gorilla Tag so you can just click the `Save & 
next` button at the bottom of the page to continue. You'll then be taken to the overview page for your map. At the top of this page 
are 2 important things to be aware of: Level Status and Level Visibility. 

- Level Status refers to the approval status of your map. All maps must be verified and approved by a Gorilla Tag moderator before
  they will be available to players in-game or on the mod.io website.
  In order for a map to be approved, it must go through an approval process. For more information, please visit the Community Modding Discord here: https://discord.gg/mzTFwPRhQ5
- Level Visibility refers to the public visibility of your map. If it's hidden it will only be available to pre-existing subscribers
  and anyone added to the map's Team.

Your map must be approved and visible to the public for players to be able to see and download it in-game. 

![status](https://github.com/user-attachments/assets/e2e44113-78c2-4350-be18-ccf2b13fadcc)

If you'd like to see what other players see when browsing mod.io, you can click the `View Level` button on the top-right of this 
page. You can also subscribe to your map on this page, just make sure to link and use the same mod.io account in-game or 
subscriptions on the website won't show up in Gorilla Tag. 

![subscribe](https://github.com/user-attachments/assets/4d89d3b3-3985-488d-b6d4-4f8f7b3f506f)

## Playing Your Map
Once you've created an entry on mod.io, uploaded your map, and subscribed to it on the mod.io website you're now ready to test it 
out in-game. Keep in mind that only you and members of your map's Team on mod.io will be able to see the map in Gorilla Tag until it 
is approved by a moderator and you must subscribe to it on the website before being able to see it in-game. 

  1. Launch Gorilla Tag on Steam/Quest and head to the Arcade which can be found in the City area.
     ![tunnel-to-arcade](https://github.com/user-attachments/assets/971da632-356a-4e45-8b25-95eefcde6f98)
     ![ramps-to-arcade](https://github.com/user-attachments/assets/b87e0624-54ba-4566-8fa0-1cc6988c16ff)

  3. Once in the Arcade, locate the green VR Game Machines and put your face up to the goggles on one of them. You'll see a short
     countdown before being automatically logged in to mod.io using your Steam/Oculus account and sent to the Virtual Stump.
     ![vr-machines](https://github.com/user-attachments/assets/e5bc9ef9-f9e0-4918-87e1-6cd438c23110)

  5. Once in the Virtual Stump, you'll need to approach the `Mod.io Account Options` screen and press the `LINK MOD.IO ACCOUNT`
     button to login with your pre-existing mod.io account (the same one you used to upload your map).
     ![account-options-terminal](https://github.com/user-attachments/assets/8a654942-c709-4af7-af5f-0a5f1896d70b)

  6. After successfully linking your account you can approach the large screen next to the door called the Maps Terminal. Press the
     `TERMINAL CONTROL` button to take control of the terminal and you'll see a list of all the Approved/Public maps that are
     available.
     ![maps-terminal](https://github.com/user-attachments/assets/2fcd4c6a-88d8-4324-b12f-9f5456e64247)

  8. Press the `OPTION` button to switch the view to `SHOW INSTALLED MAPS ONLY` and you should see your map in that list as long as
     you are subscribed to it on the mod.io website.
  9. Press the `SELECT` button to load your map and the doors should open to it once loading is finished.

If your map doesn't look quite right in-game, read back over the [Lighting section](#lighting)

---

This work is licensed under a
[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License][cc-by-nc-sa].

[![CC BY-NC-SA 4.0][cc-by-nc-sa-image]][cc-by-nc-sa]

[cc-by-nc-sa]: http://creativecommons.org/licenses/by-nc-sa/4.0/
[cc-by-nc-sa-image]: https://licensebuttons.net/l/by-nc-sa/4.0/88x31.png
[cc-by-nc-sa-shield]: https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg
