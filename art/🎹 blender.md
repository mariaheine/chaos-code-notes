## contents
- [🍰 Important Settings](#🍰-important-settings)
- Blender Modes
  - [🔭 Everywhere](#🔭-everywhere)
  - [🏔️ Object Mode](#🏔️-object-mode)
  - [🛠️ Edit Mode](#🛠️-edit-mode)
    - [🛠️🕸️ Selection](#🛠️🕸️-selection)
    - [🛠️🧩 Editing](#🛠️🧩-editing)
- [Modifiers](#)
- [🪢 Constraints](#🪢-constraints)
- [💃🏻 Animation](#💃🏻-animation)
- [🌠 Shader Editor](#🌠-shader-editor)
- [🥏 Physics](#🥏-physics) 
- [🎨 Compositing](#🎨-compositing) 
- [📸 Rendering](#📸-rendering) 
- [🔌 Plugins](#🔌-plugins)
- [🎨 painting](#🎨-painting)
- [🖼️ UV Editing](#🖼️-uv-editing)
- [🥐 texture baking](#🥐-texture-baking)

## 🔎 tutorials

> When people say shortcuts using mac devices and they say 'option', it means 'alt' for you on windows.

- The little house cycles texture bake tutorial [Baking Multiple Textures onto One Map | No Plugins](https://www.youtube.com/watch?v=9airvjDaVh4)
  - Slightly dated
- Modeling a dog and their ridiculous animation tutorial [Blender 2.8 & Unity | An express Guide for making Game Assets](https://youtu.be/era0ZjWVIxo?si=ko6jgJ_gwnVThsTY)
- [I should have done this as soon as I started using Blender - My Blender interface](https://www.youtube.com/watch?v=orqt8Fw7q8o)

## 🍰 important settings

### Default scene

- Open/Create a desired default scene
- `File → Defaults → Save Startup File`
- Click "Save Startup File" in the popup that appears.

### Viewport Shading -> Matcap

- Enable `Cavity` and set Type: `Both` 
  - For sculpting might be better off cos it exaggerates edges
- Crank up all four values for World Space and Screen Space to `2` (default 1)

### Viewport Overlays

- probs disable floor
- can enable wireframe there!
- `Faceport Ornientation` great for finding accidentaly flipped faces, remember to turn off `Backface Cullling` in Viewport Shading

### Settings -> Navigation -> Zoom

- Enable `Zoom to Mouse Position`, otherrwise it will be zooming to the screen centre

### Settings -> Themes -> 3D Viewport

- Scroll down and set `Vertex Size` and `Edge Width`
- Set colours for `Object Selected` and `Active Object`
  - Same for `Vertex Select` and `Edge Select` and `Face Selected` 
  - Also `Active Vertex/Edge/Face`

### Settings -> Keymap

- Object Mode -> Move -> Disable
	- This causes to be able to move objects around in object view with right mouse click drag and it is completely useless and when working with unity this will cause you to accidentally move objects around since in unity you look around with right click drag and your muscle memory will keep being confused when switching between blender and unity window

## 🙈 exporting

This is still some bullshit nightmare. At the moment I am doing:
  - make sure to `Apply - All Transforms` (make sure you don't do the apply for Object - Rigidbody)
  - right click on hierarchy root - `select objects`
  - use the machine plugin to export, meaning the MACHIN3 tab in object view
  - this is ok for most of the meshes

🔥😭 [UNSOLVED] The cursed conjurer mines problem: 
  - Despite applying transforms.
  - Applying Remesh modifiers don't help.
  - There has to be an additional cursed copy of the conjuerer mines object.
  - It is suffixed `HIDE ME`
  - Since it ALWAYS ends up oddly scaled after exposring to Unity.
  - The other copy will be properly scaled, work on it.
  - But if you hide or delete the original one, it will "inherit" the scale curse and it will be incorrectly scaled.
  - This is some bullshit I've spent some hours looking for a solution and I give up, a curse bearing copy hidden ghost is FINE by me.

## 🔭 everywhere

### circle menus & cursor

+ `Shift + S` - 🏹⭕ circle menu: cursor AND ORIGIN positioning cursor
+ `Shift + C` - Reset cursor location
+ `~` - 🎯⭕ circle menu: view direction
  + `~ + 3` - view selected (sets orbit around it) 
+ `Z` - 🌲⭕ circle menu: shading

### screen splitting
+ go to a window edge untill cursor becomes a cross
+ press LMB and drag
+ to close window right click on the border between and press `join areas`

### common

+ `A` - select ALL
  + `double tap A` - deselect ALL
+ `H` - 🕶️ hide selected
  + `Shift` + `H` - hide all unselected
  + `Alt` + `H` - unhide all hidden
+ `Shift + R` - ✨ repeat last action

### ui

+ `F3` / `space` search menu
+ `Tab` - Toggle object / edit mode
+ `N` - viewport tab
+ `M` - 🗃️ move to collection 
+ `Shift + A` - new object menu
  + 🌟 `Alt + F9` - reopen the Add menu if you closed it by accident
+ `F12` - quick preview render
+ `Ctrl + Alt + Q` - 🙈 Toggle quad view
+ `Alt + Shift + Z` - 👗🎨🌲 Toggle all gizmos, nice for taking in what ur working on

### viewing

+ `Numpad .` / ✨ remapped 2 `Shift + F` → ✨ Focuses the selected object or selection in the 3D Viewport.
  + If you're in Camera View, this will move the camera to frame it.
  + Remap `Frame Selected` in Keybindings, common to remap to something, `F` for Focus would be nice, but is used in Edit mode to create faces.
+ `Num5` → Toggle Ortho / Perspective
+ `Shift + tilde but not tilde but the quotation kindof mark`
	+ GAME WALK VIEW
	+ useful in caves
### outliner
+ `Ctrl + F2` - batch rename objects

## 🏔️ object mode

+ `ctrl` + `A` - 🗃️✨🎯 apply menu, useful, has apply transforms
+ `ctrl` + `P` - (on multiple selected objects) opens up set parent menu, the parent will be set to the last selected object
+ `ctrl` + `J` - join objects together into one
+ `W` - simplified object menu
  + has apply shading
  + has set origin

## 🛠️ edit mode

### 🛠️🕸️ selection

#### cool ✨🕶️

+ `Shift + G` - select faces similar to the current selection
  + tip! experiment with this, it can give some interesting ideas for editing
+ `<select two faces> + SHIFT + CTRL + NUMPAD +/-` - expand/shrink selection by pattern between two initial selections
  + tip! useful for selection of every second face in some loop
+ `Shift + B` - box-selection with zoom
#### basics
+ Additive / Path
  + `Shift + click` - additive selection
  + `Ctrl + click` - shortest path selection
+ `Ctrl + I` - inverse selection
+ Expanding / Shrinking
  + `Ctrl Numpad+` - extend selection
  + `Ctrl Numpad-` - shrink selection
+ `Shift + B` - box-selection with zoom

#### loopz
+ 🌟 `Alt + click` - edge loop selection (KEY!)
  + 🍰 Important! The loop will go in direction of the edge you are closest to when clicking!
  + `... + Shift` - toggle additional loops into current selection
  + `... + Ctrl` - instead select edge rings
  + tip! if your importent model can't loop selected check if it doesnt have a ton of duplicate vertices, this can be fixed with `Mesh - Cleanup Menu`

#### modes
+ `1,2,3` - alternating through vert, edge and face selection modes
+ `B` - box selection
+ `W` - toggle through selection modes
  + `Tweak`
    + `CTRL + RIGHT C&D` - additional lasso selection without switching general selection mode (add `SHIFT` to that and that lasso will be de-selectiong)
  + `Box`
    + `SHIFT + DRAG` - Add to selection
    + `SHIFT + click` - Toggle selection of hovered element
    + `CTRL + DRAG` - Remove from selection
  + `Circle`
  + `Lasso`

### 🛠️🧩 editing

#### TL;DR
+ `Ctrl + A` - apply transformations (like scale and rotation)
+ `Shift + Numpad 1,3,7` - align view to the normals of selection
+ holding `shift` while dragging will make tiny distance changes

#### essential 

+ `G, R, S` - basic tools: move, rotate, scale
+ `Ctrl + R` - ✨ loop cut
  + scrollwheel for additional cuts
+ `Ctrl + A` - ✨ apply transformations (like scale and rotation)
+ `Ctrl + B` - add edge  bevel to selection
  + `Ctrl + Shift + B` - bevel vertices
+ `Shift + Numpad 1,3,7` - align view to the normals of selection

#### tools

+ `M` - merge tool
  + there is also an `Auto Merge Vertices` toggle button to handle vertices that are really close to each other
+ `I` - inset tool
+ `E` - extrude tool
  + `Alt + E` - extrude faces individually

## 🎨 vertex paint

- they are using `Color Attribute` 
	- you can access it or create a new one in `Data` tab of your selected Object
	- when creating a new one initalize it with black color
	- otherwise you will have some leaks in your texture blending shader
- to make it visible
	- switch to `Solid` shading
	- open Solid shading options
		- in `Color` tab set it to `Attribute`
	- in `Show Overlays` tab
		- set `Wireframe` enabled to make vertex painting easier
## 🌠 shader editor

+ after `shift-A` press `S` to search for node
+ `ctrl` + `alt click` + `drag` - 🔪 knife that cuts node connections

## 🪢 constraints
+ There are noeat various constraints to map rotation, location in reference to another object
+ `hOW DO i KEEP A LOCAL OFFSET TO ANOTHER OBJECT IN BLENDER?`
```
Select the object you want to follow another (the child).

Go to the Constraints tab (🔗 chain icon in the Properties panel).

Add a "Child Of" constraint.

Set the Target to the object you want to follow.

✨🌄 Click the "Set Inverse" button.

This keeps the current local offset when the constraint is applied.
```

## 💃🏻 animation
+ ✨ press `I` while `hovering over value` to keyframe it

## 🥏 physics

+ instead of trying to bake certain physics setting, animate and cache the process of getting there, this way you can also choose wwhich frame looks best to you
+ use `Vertice groups` to bind immobile places in cloth simulations

## 🎨 compositing
+ press `Use Nodes` to begin
+ use `Viewer` to preview result in background
    + `V` key to zoom out

## 📸 rendering
+ In outliner `untick the 📸 icon for stuff not to be rendered`
+ Camera positioning
  + `0` will position view to what camera looks at
  + `Ctrl + Alt + Numpad 0` ✨ position camera to view
  + `N -> View > Lock Camera to View` ✨ camera will follow view, neat
+ Rendering with transparency
  + `Render -> Film -> Transparency`
  + Choose `.png` as output file format
+ Rendering keys
  + `f12` render frame
  + `ctrl + f12` to render animation
+ Important `Render` settings
  + `Render -> Device` Set to `GPU Compute` from `CPU`
    + GPU needs to be enabled in preferences
  + `Render -> Film -> Transparent`
  + `Render Samples`

| Quality      | Min Samples | Max Samples | Noise Threshold |
|--------------|-------------|-------------|-----------------|
| Fast Preview | 8–16        | 64–128      | 0.1–0.05        |
| Medium       | 16–32       | 128–256     | 0.02–0.01       |
| High Quality | 32–64       | 512         | 0.005–0.001     |

+ Important `Output` settings
  + Frame Start and End
  + `File Format` saving `animation to an image sequence` (png or jpg) is much faster and usual workflow
    + `png` is the one that will allow for transparency aswell

### putting the image sequence together
+ if you skipped frames while rendering the image sequence in blender you might need below powershell script before running the ffmpeg command
```
$files = Get-ChildItem -Filter "*.png" | Sort-Object Name
$i = 1
foreach ($file in $files) {
    $newName = '{0:D4}.png' -f $i
    Rename-Item $file.FullName -NewName $newName
    $i++
}
```

### gif from image sequence with ffmpeg
```
ffmpeg -framerate 10 -i %04d.png -filter_complex "
[0:v]scale=720:-1,setpts=PTS/1.0,split[scaled][copy];
[scaled]palettegen=max_colors=32:stats_mode=diff[palette];
[copy][palette]paletteuse=dither=bayer:alpha_threshold=128
" -gifflags -offsetting -r 10 -plays 0 out.gif
```
- Reducing the size:
  - `640,480,360 for scale` has the greatest impact on size
  - `-framerate 8` lower numbers, less frames, lighter gif
  - `max_colors=64` can be 32,64

### webp alternative to above
```
ffmpeg -framerate 10 -i %04d.png -vcodec libwebp -lossless 0 -compression_level 4 -q:v 75 -loop 0 -pix_fmt yuva420p out.webp
```

## 🖼️ UV Editing
+ UI Button:  `UV Sync` - Important toggle button that will sync ur uv selection with what you see in 3D View
  + It currently looks like two arrows pointing in opposite directions.
  + Tip! `You have to turn it off to move vertices that run along a seam separately!`, when off:
    + UI Button: `Sticky Selection Mode`, allows you to sync selected vertices in UV map, but only when you have both faces using that vertex selected in 3D View! If only one face is selected this mode does NOTHING.
+ `ctrl + p` - pack islands (for example when smart unwrap does not take whole space)
+ `Constrain to Image Bounds` - a useful checkbox in uv windows uv tab, then u can easily scale things but they will be clipped to the uv square image

## 🥐 texture baking
> Useful to bring textures from a high poly model to a low poly one. Or from multiple objects into one.
1. Position ale the target objects to be overlapping perfectly at origin.
2. First select all the source objects then last select the target object.
    - All of them need to be visible!
    - 🎨 In the target object assign a new material and set its base color to `Texture Image` and create new image assigned to it. Seletct desired dimensions nad tick `32 Float`
3. Go to rendering (camera icon) menu and switch to `Cycles` renderer.
4. Go to `Bake` section.
5. Settings:
    - `Bake from Multires` used to bake detail into normal/displacement maps, cool!
    - Otherwise bake `Diffuse` if you want color.
    - `Selected to Active -> Extrusion` (in meters) controls how far rays are cast from the low-poly to high-poly surface
    - `Margin (Size)` (in pixels) controls how much texture is baked beyond the UV islands
6. Press bake! 📸 

📸🔥 That image is only in memory will be lost after quitting blender! Go to `UV Editing` and choose one of the:
- `Image -> Save As` to save as external file
- `Image -> Pack` to pack it into the .blend project file

### 🥐 bake leaks!
![Bake Leak Example](./images/blender-bake-leak.png)
- Temporary separate parts of the `target mesh` that get leaks onto each other
- 🔥 Before baking to them from the source mesh, make sure the corresponding `parts of the source` whose shadows you separated in before step are not selected while baking to the other parts.
- Bake them separately, but bake to the same texture! 
  - They must be properly UV unwrapped before so they don't overlap and will nicely fit together when rejoined.
  - Remember to untick `Clear Texture` on consecutive bakes.
- Simply rejoin meshes, they already have a shared texture.

## 🎨 painting
- `F` set size

## 🔌 plugins

### Writing a custom plugin!

- Use VSCode
- U need `Blender Development Tools` or smth like that vscode extension
	- https://marketplace.visualstudio.com/items?itemName=JacquesLucke.blender-development

- `blender --command extension build` to build
  - this will use version in plugin manifest `.toml` file
- Open proj in vscode and command `Open in blender` or smth like that

#### Virtual Environment
- `python -m venv .venv` creating one
  - `-m` This flag tells Python to run a module as a script. Instead of running a standalone Python script, you're telling Python to execute a module. In this case, you're running the venv module, which is a part of the standard Python library starting from Python 3.3.
- `.venv\Scripts\activate` activating it (onw windows)
- `deactivate` to deactivate

#### Dev Environment
- https://www.youtube.com/watch?v=q06-hER7Y1Q
- https://github.com/JacquesLucke/blender_vscode
- https://github.com/nutti/fake-bpy-module
  - `pip install fake-bpy-module`

### Free And Great
- [Bool Tool](https://extensions.blender.org/add-ons/bool-tool/?utm_source=blender-4.4.0)
  - `ctrl + numpad -` quick difference between two selected objects
- [Magic UV](https://docs.blender.org/manual/en/latest/addons/uv/magic_uv.html)
   - makes it easy to set a unified scale for environment meshes
   - `U - World Scale UV - Apply Manual 100`
- [Fast Carve](https://blender-addons.org/fast-carve/)
- [3D Print Toolbox](https://extensions.blender.org/add-ons/print3d-toolbox/?utm_source=blender-4.4.0)
  - `N` panel
  - `Check All` will say if some nonmanifold
  - In `Clean up` has `Make Manifold`
- [`Modifier List`](https://extensions.blender.org/add-ons/modifier-list-fork/?utm_source=blender-4.4.0)
  - added through extensions menu
  - makes modifiers list more readable when there is a lot of them
  - lets you copy entire stacks of modifiers on other objects
- Images as Planes
  - ✨ Replaced by native `Shift+A -> Image -> Mesh Plane`

### [MACHIN3 tools](https://machin3.gumroad.com/l/MACHIN3tools)
- Tutorials:
  - [Every tool I use in Machin3 Tools explained for Blender](https://www.youtube.com/watch?v=P_3yh2m-uw4)
    - TODO: coontinue on from the `Align` videoo part
  - https://www.youtube.com/playlist?list=PLnqmLZKRm5CYGKLbXGa3l74XPxTi45ft2
- Some of it is free (pie menus)
- Useful pie menus:
  - `Modes Mode` - `Tab` can quickly enter specific face/edge selection mode from the object mode
  - `Cursor And Origin` - `Shift - S`
  - `Tools` - `Q`
- `Punch It` is not free
  - [Punch It is so much better than native Blender for manifold extrusions](https://www.youtube.com/watch?v=-6-ljYBWCUk)
  - `Hold E while hovering an edge` - snap to the edge!
  - `W` Push & Pull moves the sides of extrusion (intrusion xd?) on the second stage of punching, this can help when there are slight differences in angle you you get the kindofzero-thickness walls after  the initial intrusion 
- Smart Edit Modes (since you are using `modes pie` you can use 1,2,3 keys better)
  - `Smart Vert` - replaces `1` vertex merge at last
    - `Shift 1` merge at center
    - One Selected: Bevel
  - `Smart Edge` with `2`
    - In vertex mode:
      - Selected verticves: Will create edges between.
      - No Selection: Knife tool
    - In edge mode:
      - No Selection: Loop cuts
    - In face mode:
      - Selected faces: Will switch to selected edge outline (back and forth)
   - `Smart Face` `4`
      - In vert mode:
        - Creates new faces in a smart way, `with only one vertex selected!`
      - In face mode:
        - Selection: Copies faces, detaches, enters edit mode in the new object.
  - `Clean Up` - `3`
    - Cleans up near verts



