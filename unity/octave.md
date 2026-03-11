## Feature requests

- Make deletion option more sensitive to just delete one object, its so easy to delete everything that is behind it
- Aligning (rotating and sizing) grid to the selected object so that it is possible to work in non-straight angles, to avoid everything looking so quarish

## 🧙🏻‍♀️ Basics

- `Alt + R` toggle the tool object
- `1` for object placement mode
- `2` for selection modex
- `3` for delete mode

## 🛒 Grid

- `CTRL + SHIFR + MOUSE WHEEL` - free movement vertical offset
- 🌟 `HOLD G + CLICK (on object in scene)` - raising placement grid to that height
    - 👾 This but `double click` - set placement guide to the `bottom` or `up` of the object `depending on the vertical half pressed of that object`
    - `[ & ]` grid up or down
 - 🌟 `K` - sets the grid XZ cell size to the XZ size of the object placement guide;

## 🧙🏻‍♀️ Placement Mode (1)

- 🌲 Crucial:
	- `Alt + click` -> select hoevered prefab
	- `S` -> toggle Object-To-Object snapping vs. Modular Snap
	- `SHIFT + C` -> toggle grid snap climb
- 🎡 Rotation:
	- `Shift + mouse drag` rotation around up
	- `X / Y / Z` -> rotate around exes
	- `I` -> reset rotation
- ⚖️ Scale:
	- `ctrl + drag` scale
	- `o`  reset scale
- ⬆️ Offset:
  - `q` - down
  - `e` - up
  - `r` -> 🌲 reset vertical offset
    - `ctrl + shift + mouse scroll` -> to change smoothly vertical offset
- Highlight Hints:
	- `Space` -> toggle alignment highlight
	- 🐛 `Shift + Space` -> toggle alignment hints 
- `SHIFT + F + SURFACE CLICK` -> align selected prefabs along the pressed surface
- 🐛🌟 `J`  toggle pivot point
- 🌟 `U` -> object 2 object snap
- 🐛 `W` -> toggle object surface snapping (also a checkbox in snapping window)
- 
## Display

- `SPACE` -> toggle align highlights
- `SHIFT + SPACE` -> toggle lable displayes
- `ESCAPE` -> kill the spawn guide
 
## 🕸️ SELECT MODE (2)

- confirmed:
- `SHIFT + A` -> select all prefabs of the selected type
- `SHIFT + V` -> filter out objects from selection that are out of view
- unconfirmed:
- `(SELECTED OBJECT IN SCENE) ALT + SELECT NEW PREFAB` -> prefab replace
  - do the same for multiple selected objects, but select multiple replacing prefabs and then to the alt+click to randomly replace
- W, E, R - transform gizmos are available in selection mode
- `G` - select all prefabs of the same type as selection
- (what?) Buttons:
    - `Assign or Remove selection from snap mask` - pressing this will disable (assign) or enable (remove) objects from snapping to selected prefabs 

## 🌲 Prefab Management

- just drop prefabs into lib manager
  - To change what happens then press `Prefab Drop Settings` button
- There is checkbox to spawn prefabs as `static`


### Mirroring
- `CTRL + Q` -> toggle mirroring guide

### Extensions
- SPACE – only works with the PointAndClick, Path and Block placement modes. 
  - When this key is pressed, the tool will always snap the center pivot point (regardless of the active pivot point) to the center of the snap surface
- LCTRL - only works with the PointAndClick, Path and Block placement modes. 
  - When this key is pressed, the tool will attempt to keep the object placement guide inside the area of the snap surface.

### Essential
- `Hold Ctrl + drag` "the tool will attempt to keep the object placement guide inside the area of the snap surface."
  - in other words small changes in current placement position
- `Longpress K` set grid size to the currently held prefab
- `Q + horizontal drag` offset placement along normal
- `LShift + LCtrl + horizontal mouse` uniform scale
- `U` - toggle object to object snapping (more settings under magnet icon)
  - larger epsilon for larger models (0.5 for small )

## Object Spawn -> Curve

- You can only add prefabs to the curve window from the `Prefab Manager` window.
