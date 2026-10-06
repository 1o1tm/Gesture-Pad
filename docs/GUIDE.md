# Gesture Pad

Touch gestures, camera control, guest sign-in, zone select and drag-and-drop on any Q-SYS UCI. One plugin, nine modes.

| Mode | What it's for |
|---|---|
| PTZ Pad | Drag to aim a camera. Double-tap = home |
| Joystick | Speed control for PTZ. Springs back when you let go |
| Camera Framing | Draw a box around the next shot (or drag a corner); the camera pans, tilts and zooms to it. Double-tap = zoom out, Home = the room's shot |
| Dial | A big jog wheel for zoom or any knob (volume, a gain, a level). The center is a dead spot so it never jumps |
| Swipe Layer | Swipe left / right / up / down, tap, double-tap, hold |
| XY Pad | Raw X / Y for anything |
| Sign-In | Guest signature, saved on the Core with a CSV log; can also post to a Teams channel or any webhook |
| Zone Select | Tap, paint across, or lasso zones on a map (paging zones, divisible rooms, mic groups) |
| Drag & Drop | Drag sources onto displays (or a destination you call "Teams Call"), or tap a source then a screen; double-tap a source to send it to every screen; sources without signal fade. Up to 64 sources and 64 screens, on pages when they don't all fit |

## What has been tested, honestly

- **Designer Emulate:** the demo design, every page, by hand (0.9.3 in 10.5; 0.9.2 in 10.0.3 and 10.5), and three of the five job designs below in 10.5.
- **On hardware:** one tester ran 0.9.3 on a Core 110f and a TSC-101-G3 with Q-SYS 10.3 (XY Pad mode). It worked, at about 0.2% CPU and 0.4 to 1.1 MB of memory (the swing is Lua's garbage collector; it doesn't grow over time). Their three notes on gestures are fixed in 0.9.4.
- **An offline test harness** that fakes Q-SYS and enforces the Core's script rules (the 200,000-instruction limit per call, no `os.clock`): 361 scripted tests, a fuzzer, a layout linter and a budget check, and for Drag & Drop 2,678 checks over eleven made-up jobs (1 to 64 sources and screens), with every label measured against its tile in Roboto.
- **Five made-up jobs:** a school paging console, a boardroom, a lecture-hall camera desk, a restaurant's music zones and a lobby visitor kiosk were each built as a whole design around one Gesture Pad, to find what is awkward in real use. What they found is fixed in 0.9.3 or written down below.
- **The touch panel side was worked out from QSC's own code**, the UCI viewer that ships inside Designer (how a TSC draws the Color Picker, which controls let touches through, how it decodes the drawing). Only XY Pad has been tried on a panel so far. One try of an older version (0.8.9) on a Core and a TSC-50 failed in several ways; all of that was fixed in 0.9.0.
- **Cameras have only been tested against simulated cameras.** The Teams card and webhook only against a fake web server.

So: please try it, break it, and tell me what happened.

## How to set it up

**Fastest way to try it:** open `Gesture Pad demo.qsys`, press F6 (Emulate), open the UCI. Every page is already wired. `Gesture Pad single pad.qsys` is the smallest UCI to try on a touch panel first (one page: the joystick with the demo camera).

### Every mode: the same steps

The plugin block on its own does nothing on a panel. A Color Picker feels the finger; the plugin reads it and draws on top.

**The short version** (each step is explained below):
1. Block: pick the Mode and the pad's size.
2. Color Picker: Script Access All; type its Code Name into the block's Color Picker property.
3. Read the block's Setup page: the picker's size, and where the pad can go.
4. UCI: the picker surface at that size and place, sent to the back.
5. A Group Box over the picker, page colour, 2 px bigger.
6. Where the pad goes: a box in the pad's colour, then the Display from the block's **Display** page pasted 3 times on top.
7. UCI: Swipe Disabled on.
8. On a TSC: tick and wire Touch Activity to the block's Panel Touch.

1. **Block:** drag Gesture Pad into the schematic. Pick the **Mode** and set **Pad Width / Height** (e.g. 500 x 500; 16:9 such as 640 x 360 for Camera Framing). On a 1280-wide UCI keep the pad's longer side at about 640 or less: the picker behind it is 1.91 times as wide and the longer side + 24 tall (step 4).
2. **Picker:** add a **Color Picker** (Control Components), set its **Script Access** to **All** (or Script), and type its **Code Name** (not its label) into the block's **Color Picker** property. **Each Gesture Pad needs its own Color Picker.**
3. **Where the pad goes:** the picker reaches out to the right of and below the pad, so the pad can't go just anywhere. The block's **Setup** page shows where its top-left corner fits on a 1280 x 800 page: for a 640-wide pad that's x 49 to 106, the left side of the page. Leave room for a tab bar.
4. **Picker on the UCI:** copy the picker's color surface onto the page at the size and position the block's **Setup** page shows ("Picker on the UCI: W x H, top-left ... left and 12 above the pad's top-left"). For a 500 x 500 pad that is **955 x 524**, its top-left **38 left and 12 above** the pad's. **Send the picker to the back** (lowest in the layer list). Everything else on the page sits on top of it: buttons and fields there keep their own touches, and a touch that lands on the picker outside the pad is ignored, even if it then slides onto the pad.
   Why so big: touch panels (TSC-G3) and browsers draw the picker's touch area as a **square** (the side is the smaller of 4/7 of 92% of its width and its height minus 24), while Designer draws a wider rectangle. This size puts the square on the pad and covers the pad in Designer too; the plugin knows both shapes, so neither should need calibrating.
5. **Cover it:** a **Group Box** over the whole picker and 2 px bigger on every side (or the picker's edge shows as a thin line): Fill = page color, Stroke Width 0, Corner Radius 0, just above the picker in the layer list, under everything else. Touches pass through graphics.
6. **Drawing:** open the block's **Display** page and copy the **Display** there: it is already the pad's size. Paste it onto the UCI **3 times**, exactly over the pad. The Display is disabled so touches go through it to the picker, and a panel draws a disabled control at 35% strength: 2 copies come out at about 58%, 3 at about 73%, 4 at about 82%. (Designer 10.5 shows the same fade in Emulate.) Under the copies, put a box in the pad's own colour (the Display page says which), so the faded drawing looks solid on a panel: see "Making it look right on a panel". Tip: group the picker, the cover, the box and the copies, so the pad moves in one go.
7. **Swipe off:** select the UCI itself and turn on **Swipe Disabled** in its properties. Otherwise a sideways drag on the pad flips the UCI page (on a TSC and in the UCI Viewer apps) instead of reaching the picker.
8. **Test:** save, F6, drag on the pad. On a TSC, also wire the panel's **Touch Activity** to the block's **Panel Touch**: tick **Touch Activity** under Control Pins on the TSC's Status/Control block and **Panel Touch** on the Gesture Pad, then wire them (see "Touch Activity" below).

**Not working?** Open the block's **Setup** page and read **Status**. It names the problem: no picker, a picker that can't be found (use its Code Name; check its Script Access), a component that isn't a Color Picker, a Dial "Drives" target whose component or control can't be found. Camera problems show on the camera's own status. If Status says OK but nothing moves, something other than graphics is on top of the picker, or the UCI has a different picker than the block. For more detail set the block's **Show Debug** property to **Yes** and **Debug Print** to **Gestures** (each touch and gesture) or **All** (every picker report too), then read the block's Debug Output. If Status says **Recovered from an error: ...**, the pad kept working but something went wrong: the full text is in the Debug Output; please send it to me.

### Calibration

Only needed if the drawing doesn't line up with your finger (a different picker layout, another viewer). It's a setup tool: keep **Calibrate** off the pages end users see. Setup page: **Calibrate** on, tap the two targets (taps only, on each target; it gives up after a minute untouched). The result is kept per screen type: `P ...` applies on panels and browsers, `D ...` in Designer, so calibrating one doesn't upset the other. It's lost on the next deploy unless you type it into the design (the **Calibration** field on the Setup page). Clear the field to go back to the built-in layouts.

### Touch Activity

Without it the plugin guesses a lift from silence (**Release** on the Setup page, 0.25 s). That works, with these limits:
- **Hold / long press needs it.** A finger resting still sends nothing, so without Touch Activity the plugin can't tell a held finger from a lifted one.
- A drag that pauses longer than the release time looks like a lift for a moment. The plugin copes: a lasso or a framing box is applied about 1.75 s after the finger stops (0.25 s to notice, then 1.5 s in case the drag carries on), and a drop that turns out to be a pause is taken back.
- Very quick taps are paired up by position and timing; a tap held still for a moment and then rolled off can read as a double tap.

With Touch Activity wired, press and release follow the pin. It's screen-wide, so: **one panel per pad.** If the UCI is on more than one panel, OR their Touch Activity pins together (Logic components) into Panel Touch, or leave it unwired. A browser or the UCI Viewer app has no Touch Activity: its touches still work, with lifts guessed as above.

### Touch panels

- **One finger at a time per screen.** The panel's viewer follows only the first finger; a second finger can cut the first drag short. Two pads on one page take turns.
- **Smoothness:** the plugin redraws at most 20 times a second by default. **Max Frame Rate** in the block's properties (10, 15, 20 or 30) changes it; it's a property, so change it in Designer and push again. A TSC-G3 checks for changes 30 times a second, so more than 30 never helps.
- **Q-SYS version:** the drawing is a button icon set from the script (the same technique as QSC's Dynamic Icons and QR code examples). QSC fixed TSC-G3 button icons that could stay on a loading spinner in **10.2** and an icon refresh bug in **10.4**, so **10.4 or later is recommended** (not yet tried on any version on a panel).

### Making it look right on a panel

- A touch panel draws the pad faded, and so does Designer 10.5 in Emulate, because the Display is disabled (that is what lets touches through to the picker): 3 stacked copies come out at about 73%, so 27% of whatever is under them shows through. Over a light page or the wrong colour, the pad looks washed out. A single copy comes out at 35% and looks very faint.
- So put a box in the pad's own colour right under the copies, the pad's size. What shows through is then the same colour, and the pad looks solid. The block's **Display** page names the colour for its mode:
  - PTZ Pad and XY Pad: 28, 35, 45, corner radius 18
  - Drag & Drop, and Zone Select with its background on: 20, 25, 32
  - Sign-In: white, corner radius 18
  - Joystick, Dial, Camera Framing and Swipe Layer draw on a clear background: put your own background there.
- On a light page, keep the cover in the page colour and only the box under the pad in the pad's colour. The pad's colours are fixed (dark) for now.
- Designer 10.5's Emulate shows the fade too, so the look can be checked there; the panel has the final say.

### Designer notes

- Emulate redraws the UCI page lazily: the pad can look a second or more behind. The plugin itself reacts in about 30 ms (Debug Print shows it). The block's own panel updates sooner than the UCI tab.
- Designer has no Touch Activity, so the limits above apply: hold doesn't work with a still mouse, and lassos and framing boxes are applied about 1.75 s after the mouse stops.
- Tapping exactly the same spot twice in a row is reported only once (Designer doesn't resend a value it already sent), so a mouse double-click can't test a double tap: click twice a few pixels apart. Touch panels send every touch.
- Designer 10.5 redraws the UCI far faster than 10.0.3.
- Sign-ins in Emulate go to Designer's temporary folder and are gone after the session.

### Then, per mode

**PTZ Pad / Joystick / Dial / Camera Framing (cameras)**
- Set **Camera Control**: **Demo (simulated)** to try it with no camera, **Q-SYS Camera** (type the camera's name into the **Camera Name** property; the camera needs Script Access = All), or **VISCA over IP** (brand + IP on the Setup page).
- With the Demo camera, also copy the display from the block's **Camera View** page onto the UCI (3 times, like the pad's Display) to watch it move.
- Optional: Zoom +/- from the **Pad** page, and Home and Flip on PTZ Pad, Joystick and Framing. Set **Wide angle / Optical zoom** on Setup to the camera's spec: aiming, framing and the dial's zoom readout use them.
- Dial without a camera: type any control into **Drives** as `Code Name~control` (e.g. `Gain_1~gain`; names are case-sensitive; that component's Script Access Script or All), or wire the `DialValue` pin. A script can change Drives at any time (one knob for several zones: see Recipes). The knob moves the control's position, so one turn covers the control's whole range divided by **Turns**: a stock -100..+20 dB gain at Turns 2 is 60 dB a turn. Narrow the gain's range, or raise Turns, for a gentle volume knob.
- Camera Framing: **Wide** (the Reset button) zooms all the way out where the camera points; **Home** sends the camera home and zooms out, the room's wide shot. Each box zooms in on what the camera shows now (up to 4x a box at Tightest frame 0.25), so boxes add up: Home brings it back. A box smaller than 8% of the pad isn't sent (a hand brushing the panel). With the Demo camera the pad shows the room itself. With a real camera the pad draws only the box and dims the rest, so put the camera's live preview underneath in place of the Group Box (untested on a panel: tell me if the preview blocks touches). Any pad shape works; the camera's view is the widest 16:9 box on the pad. For your own camera logic, **Apply** pulses when a shot is sent, and Frame Pan / Tilt / Zoom then hold that shot (in the view it was drawn on) until the next touch.

**Zone Select**
- Set **Zones** (count) and type the names on the block's **Names** page.
- Zones start as a grid. To match a floor plan: put your floor-plan image as the cover, turn **Show Background** off, turn **Edit Layout** on, then drag a zone to move it, drag its bottom-right corner to resize it, or drag on empty space to redraw the last zone you touched there. Edit off when done; it switches itself off after 5 minutes untouched (left on, a tap moves a zone instead of picking it) and prints the layout to the block's Debug Output (Show Debug = Yes). The layout is the **Layout** text on the Names page: type it into the design to keep it.
- Wire each `Zones ~ Selected n` pin to a page zone, mute, or room-combine input. They work both ways. On means picked, so into a mute it's backwards: put a Logic NOT in between, or use a script.
- A drag that starts on a zone paints the opposite of that zone's state, so starting on a picked zone removes zones. Lasso and double-tap-to-clear start on empty space; on a dense floor plan, give people All / None buttons too.

**Drag & Drop**
- Set **Sources** and **Destinations** (up to 64 each), type the names on the **Names** page. Long lists go in columns of 16; when that makes more than four columns, the names move to a **Sources** page and a **Destinations** page. The icon follows the words in the name (laptop, PC, Teams, Zoom, camera, doc cam, Apple TV, ClickShare, mic, music).
- On the pad: drag a source onto a screen, or tap a source and then a screen (a tapped source waits 8 s, then is put down); drag a screen's source off to clear it; double-tap a source to send it to every screen. A destination whose name makes it a call (Teams, Zoom, Webex, codec, call...) is left out of "every screen": a source only goes into the call when someone drops it there.
- `Routing ~ Destination n` (Lua `Route n`, on the Outputs page) is the source number on display n, 0 = none, both ways. **QSC routers number their inputs from 1** and have no "none", so don't wire it straight to a router's output select: use a short Control Script (see Recipes). `RouteName n` is the source's name for a label (empty when nothing is on that screen).
- Optional: `Sources ~ Signal n` from each input's signal LED to dim sources with no signal. A dimmed source can still be routed, so a laptop can go on a screen before it's plugged in and come up when it is. Left unwired, every source counts as having signal.
- **Not only AV:** sources and screens can be anything you assign: people to rooms, groups to the sections of a divisible hall, mics to rooms. `Route n` is the number assigned to screen n and `RouteName n` its name, for your script or a door sign. Each screen holds one at a time; a double tap gives one source to every screen, so leave it out of the instructions where that would be a mistake; icons follow the words in the names (a room gets a screen icon).
- **Many sources or screens:** when they don't all fit at a finger's size, they go on pages, with arrows under the sources and under the screens. A page shows at most 12 sources and 16 screens, so the drawing the panel redraws stays small; a bigger pad shows more at once. To route to a screen on another page, drop the source on the screens' arrows (the page turns and the source stays picked) and tap the screen, or tap the source, turn the page and tap the screen. Double-tap still sends a source to every screen, on every page.

**Sign-In**
- Copy Guest Name, Company, Visiting, **Redo** (the signature only), **Clear All** and **Sign In** from the **Pad** page onto the UCI. **Last Guest** and **Today Count** are on the **Outputs** page.
- Sign-ins save on the Core (`media/SignIns`, with `log.csv`). For Teams, paste a Workflows webhook URL on the Setup page and set the block's **Sign-In Webhook** property to **Teams Card**.

**Swipe Layer / XY Pad**
- Swipe Layer draws only your finger trail, so the cover can be any graphics or images. XY Pad draws its own card over the pad.
- Wire `Live ~ Swipe Left/Right/Up/Down`, `Live ~ Tap / Double Tap / Long Press` or `Live ~ X / Y` to whatever you want (e.g. swipe to change UCI pages: see Recipes below). A swipe needs distance (Setup page) and some speed. A double tap also pulses Tap for its first tap (a tap isn't held back to wait for a second one), so don't give Tap and Double Tap actions that clash.

Full pin list for every mode: [PINS.md](PINS.md).

## Cameras

Set **Camera Control** in the block's properties. **All camera drivers have only been tested against simulated cameras so far.**

- **Q-SYS Camera:** the camera named in **Camera Name** (or the only camera in the design, or the one picked on the Setup page while running). The plugin finds its pan, tilt, zoom, speed, position and home controls by name, which covers Q-SYS cameras and camera plugins that use normal control names (PanLeft, pan.left, ...). It puts the camera's own speed sliders back after each move and keeps a focus value in its position.
- **Demo (simulated):** a built-in fake camera with its own Camera View page. Use it to build and demo the UCI with no camera at all.
- **VISCA over IP:** pick the brand, enter the IP (or `ip:port`). AVer, Sony, Lumens, Marshall, BirdDog and Avonic use UDP 52381 with the Sony header; PTZOptics uses TCP 5678. Speed control (Joystick) works the same on all of them. Aiming, framing and the dial's zoom need the camera's position scale: it's taken from the makers' documents for **AVer PTZ310/330, Sony BRC-X400-class and PTZOptics**; the other brands are assumed to use the PTZOptics scale, so positions may be off there. Stops are sent three times. A camera that never answers position questions shows a warning on its status.

**Flip Pan / Flip Tilt** (Pad page, copy onto a UCI or drive by pin) reverse the camera commands and output pins for ceiling mounts or a drag-the-picture feel.

Framing and drag-to-aim turn finger movement into degrees using **Wide angle** and **Optical zoom** on the Setup page, with a simple lens model: close, not exact.

## Sign-In

Each sign-in writes `media/SignIns/<time>_<name>.svg` and adds a line to `media/SignIns/log.csv` on the Core (UTF-8 with a byte-order mark so Excel shows accents; names can't turn into spreadsheet formulas). If the file can't be written, the guest sees "Not saved: please ask at reception", nothing is counted, **Submitted** doesn't pulse, the form stays, and Status and **Last send** say why. **Signed in today** counts today's lines in the log, so it survives a restart, and resets at midnight. A signature or fields left alone for about 3 minutes are cleared. A second press of Sign In right after a sign-in does nothing.

With a **Webhook URL** it also POSTs JSON (name, company, visiting, room, time, the signature as base64 SVG, and the file name), or a **Teams Card** (an Adaptive Card with the name, company, visiting, room and time, no signature) for a Teams Workflows webhook. Power Automate should be able to take either and file it in SharePoint, send email, or feed a visitor system. (Tested only against a fake web server so far.)

## Using it in your own design

Gesture Pad is a building block, not a finished system. It turns touches into gestures and values; you decide what they drive. Three ways in:

1. **Built-in drivers** for the jobs that need them: cameras (Q-SYS cameras, VISCA over IP, or the demo camera) for PTZ Pad, Joystick, Framing and Dial; zone and routing state for Zone Select and Drag & Drop; Core files and webhooks for Sign-In.
2. **Control pins.** Every output is a pin: tick it under **Control Pins** in the block's properties and wire it. Full list per mode in [PINS.md](PINS.md).
3. **Lua.** Set the block's **Script Access** to Script or All, then any Control Script or Block Controller can read and drive it with `Component.New("<code name>")`. [PINS.md](PINS.md) gives every control's Lua name (`Route 1`, `SelectedList`, `DialTarget`, ...).

Recipes:

- **Drag & Drop to a video switcher:** `Routing ~ Destination n` is the source number on display n (0 = none) and works both ways. `Routing ~ Source Name n` gives the name as text for labels; `Routing ~ Changed n` pulses on every change, from the pad or the pin. `Sources ~ Signal n` dims a source with no signal. A QSC router's inputs start at 1, so map 0 to an input you leave empty:
  ```lua
  local pad = Component.New("Share_Pad")          -- the Gesture Pad block's Code Name
  local router = Component.New("Video_Router")    -- the router's Code Name
  local BLANK = 5                                 -- a router input left empty: "nothing on this screen"
  for d = 1, 2 do                                 -- each screen
    local route = pad["Route " .. d]
    local sel = router["select." .. d]            -- the output select; list the names with Component.GetControls("Video_Router")
    route.EventHandler = function() sel.Value = (route.Value > 0) and route.Value or BLANK end
    sel.EventHandler = function() route.Value = (sel.Value == BLANK) and 0 or sel.Value end
  end
  ```
- **Dial to volume or brightness:** type the control into **Drives** on the Setup page as `Code Name~control` (for example `Gain_1~gain`); the dial moves it directly. Or wire `Outputs ~ DialValue` (0..1, both ways) or the `Live ~ Step Up / Step Down` pulses to anything. One knob for several zones: let zone buttons change what it drives,
  ```lua
  local knob = Component.New("Volume_Knob")       -- the Gesture Pad block's Code Name
  local function drive(zone) knob["DialTarget"].String = zone .. "~gain" end
  -- e.g. in each zone button's EventHandler: drive("Bar_Volume"), drive("Dining_Volume") ...
  ```
- **Zones to paging, mutes or room combining:** each `Zones ~ Selected n` is an on/off pin, both ways. `Outputs ~ SelectedList` is the names, comma separated (empty when none).
- **Swipe to change UCI pages:** put a Swipe Layer over the page, then in a Control Script:
  ```lua
  local pad = Component.New("Gesture Pad Swipe")          -- the block's Code Name
  pad["SwipeLeft"].EventHandler = function() Uci.SetPage("My UCI", "Page 2") end
  pad["SwipeRight"].EventHandler = function() Uci.SetPage("My UCI", "Page 1") end
  ```
  In Emulate, `Uci.SetPage` reports "device not found" (it needs a panel showing the UCI); try it on a panel.
- **XY Pad to anything with two axes:** `Live ~ X` and `Live ~ Y` (0..1, live while dragging), for example colour temperature by brightness, or a pan by fade.
- **Any mode:** `Live ~ Tap`, `Double Tap`, `Long Press`, `Touching` and `Gesture` are there in every mode, so even a camera pad can trigger something on a long press.

## Install

Copy `Gesture Pad.qplugx` to `Documents\QSC\Q-SYS Designer\Plugins` and restart Designer. It appears in the Schematic Elements pane under **Plugins > User > Custom**. Built in Designer 10.0.3, tried in 10.0.3 and 10.5.

**Updating a design made with an older version:** in 10.5, opening it shows **Plugin Mismatch**: choose **Open Asset Installer > Design Assets** and press **Update** for Custom~Gesture Pad. In 10.0.3, answer **Use Installed Plugin** for each block. Pins and wires keep working.

## The demo design

`Gesture Pad demo.qsys` is a complete, ready-to-push example: one UCI ("Gesture Pad Demo", 1280x800, made for a TSC-70-G3; Designer makes a TSC-101-G3's UCI 1920x1200 and a TSC-50-G3's 1280x720, so there it would be scaled: untested) with six pages. Each page has its own Color Picker + Gesture Pad pair, and every button and field on the pages is bound to a real plugin control.

| Page | Mode | Try |
|---|---|---|
| Zones | Zone Select | Tap, paint across, circle, double-tap empty space to clear (hold to solo needs Touch Activity). All / None / Edit zone layout |
| Share | Drag & Drop | Drag a source onto a display, or tap a source then a screen; double-tap a source for every screen; All off |
| PTZ | Joystick | Drive the built-in demo camera; Home, Zoom, Flip |
| Frame | Camera Framing | Draw a box around the shot; Home, Wide, Zoom, Flip |
| Dial | Dial | Turn to zoom the demo camera |
| Sign-in | Sign-In | Sign, fill in the fields, Sign in; Redo, Start over |

The cameras are the plugin's **Demo (simulated)** camera, so nothing else is needed. Switching a block to Q-SYS Camera or VISCA removes its Camera View, so that page's camera picture goes blank.

**Try it without a Core:** open the design and press F6 (Emulate).

**Push it to a Core and a touch panel:**
1. Inventory: change the Core to your model and add your touch panel.
2. In the touch panel's properties, set its UCI to **Gesture Pad Demo** (or **Gesture Pad Single** for the one-pad design).
3. Recommended: wire the panel's **Status/Control > Touch Activity** pin to each Gesture Pad's **Panel Touch** pin.
4. Save to Core & Run.
5. Drag on each page. The demo's pickers are laid out the way the Setup page says, so the drawing should follow your finger without calibrating. If it doesn't, press **Calibrate** (top right of each page, there for testing) and tap the two targets.

The demo already has the UCI's **Swipe Disabled** turned on, each drawing stacked 3 times, 44 px tabs, and every button set to its control's type (a toggle latches, a trigger pulses, zoom is held).

**Hardware status:** two real tests so far. The first, of version 0.8.9 on a Core and a TSC-50 on 10.0.3, found problems Designer's Emulate can't show: gesture timing used a clock that runs on CPU time on a Core; a script call hit the Core's per-call limit ("Max execution limits exceeded"; long lassos and design-wide searches were the likely causes); the picker layout only suited Designer (a panel's picker touch area is a square); the phone app flipped UCI pages on a sideways drag; rapid tapping froze it at times; and the Lobby (Sign-In) page sat loading. All of those were addressed in 0.9.0. The second, a tester running 0.9.3 on a Core 110f and a TSC-101-G3 with 10.3 (XY Pad mode), worked, at about 0.2% CPU; their notes are fixed in 0.9.4. 10.0.3 has a known TSC-G3 icon loading bug (fixed in 10.2), which may explain the stuck page; try 10.4 or later, and `Gesture Pad single pad.qsys` first. Please report what happened: panel model, firmware, and whether drags feel smooth.

## Changes

**0.9.7** (a code review of 0.9.6, and quick swipes)
- A double tap the pad doesn't take is a tap. Two quick taps on two different tiles pulsed Double Tap and said DOUBLE TAP, the second tap was lost, and a real double tap right after it didn't count
- Without Touch Activity, two quick taps on tiles next to each other are two taps. They could come in as one touch and send a source to every screen, or clear a screen
- Pages hold even shares: 50 sources are 7, 7, 6, 6, 6, 6, 6, 6 (it was seven pages of 7 and one of 1)
- Long names can't run a redraw past the Core's limit: each name is measured once, up to 128 letters, and remembered. With 64 sources and 64 screens named with 120 letters, a redraw needed more than the Core allows and labels could go missing; now the costliest call takes about half
- A name that isn't UTF-8 (written in Latin-1 by another script) shows with "?" for its odd letters (it showed nothing). Chinese, Japanese, Korean and emoji count a full letter width, so they stay inside their tile
- Swipe Layer without Touch Activity: quick swipes in a row each count. A second swipe within a quarter second of the first was lost, and a quick swipe back cancelled both
- Guide: Designer 10.5 shows the faded Display too (it said only a panel did); a source without signal can still be routed; a double tap also pulses Tap for its first tap

**0.9.6** (a stress test: eleven made-up jobs with up to 64 sources and screens, built as three designs and tried in Designer)
- Block pages: names and routing outputs go in columns of 16. With 64 sources the Names page was one column about 2,000 px tall, and Designer couldn't scroll to its last rows. When the columns come to more than four, the names get a **Sources** page and a **Destinations** page. Lists of up to 16 look as before
- Screen tiles: when what's on a screen (its icon, or the +) would touch the screen's name, it goes on a line under the name, on every tile alike (it overlapped on short, narrow screens). Tiles with room look as before
- Names are fitted to the width they really take in Roboto, the panel's font: no more cutting a name that fits, or running one past its tile. The tile under a dragged finger cuts its name to fit too (it ran up to 70 px past on a narrow pad), stays on the pad, and over the screens' arrows rides above them, so the page number stays in sight
- A double tap means two taps on the same tile. A quick tap on an arrow and then on the tile next to it sent that source to every screen, or routed a screen and cleared it again
- Without Touch Activity, a source drag that rests over empty space and then carries on still drops its source (it was lost)
- "Conference Room" and "Screen Share" are screens: only Teams, Zoom, Webex, codec and call make a destination a call
- Pages hold even shares: 13 sources are 7 + 6, 17 screens 9 + 8 (not 12 + 1, 16 + 1). On pads under 220 px tall, source tiles are still a finger's size
- The dim half of an arrow bar (no page that way) does nothing and catches no drop; NOW TAP A SCREEN goes when the pick runs out; a pin writing the same route again doesn't pulse Changed; a name of only spaces shows the default name

**0.9.5** (asked for on the forum: more sources and destinations)
- Drag & Drop takes up to **64 sources and 64 destinations** (it was 12 and 8)
- Sources or screens that don't all fit at a finger's size go on **pages**, with arrows on the pad under the sources and under the screens. A page shows at most 12 sources and 16 screens, so the drawing a panel redraws stays small. Layouts that already fitted look exactly as before
- A long list of sources closes up before it goes on pages; screens too small to hit get a roomier grid
- Drop a source on the screens' arrows to turn their page: the source stays picked, so a tap on a screen there routes it. Without Touch Activity, a drag that pauses on the arrows turns the page and carries on. Turning a page keeps a picked source for another 8 s
- Double-tap a source still sends it to every screen, on every page. Clear All says ALL CLEARED once; the routing outputs update only the screen that changed

**0.9.4** (from the first tester on hardware: Core 110f, TSC-101-G3, Q-SYS 10.3)
- **Live ~ Gesture** says DRAG as soon as a touch becomes a drag (it waited for the lift), and when a hold turns into a drag (it stayed on LONG PRESS)
- Without Touch Activity, a fast double tap whose second tap lands a little way from the first is a tap and a double tap. It was read as a drag, so you saw TAP, DRAG and DOUBLE TAP
- Quick taps on neighbouring zones toggle each zone (they were read as a paint from the first one)
- New **Display** page on the block: the drawing at the pad's own size, so pasted copies need no resizing, and the colour to put under them for a solid look on a panel
- README: a short version of the setup, making it look right on a panel, the tester's CPU and memory figures

**0.9.3** (five made-up jobs built around it, and what they found)
- Touch Activity: a quick second tap (0.1 to 0.2 s after the first) is no longer lost; the picker doesn't park while a new touch is being placed. A browser or the UCI Viewer app now works on a UCI whose panel has Touch Activity wired (its touches were ignored); the panel's own late reports are still dropped
- Without Touch Activity: a quick tap somewhere else (0.18 to 0.25 s after the last) waits for both of the picker's axes, so it no longer starts at the previous tap's other coordinate (it picked a wrong zone)
- A finger that lands on the picker outside the pad stays ignored when it slides onto the pad (a hand resting below a framing pad reframed the camera)
- Drag & Drop: a tapped source is put down after 8 s and by All off (it waited for the next person's tap); double-tap sends to every screen but not into a call; while a source waits the screens light up and Gesture says NOW TAP A SCREEN; 3, 5 or 7 screens fill their area (no empty quarter); icons match whole words ("Microsoft Surface Hub" isn't a microphone)
- Camera Framing: new **Home** (the room's wide shot); Frame Pan / Tilt / Zoom keep the shot that was sent, so whatever listens to Apply can read it (they were already reset); a box smaller than 8% of the pad isn't sent (a brush zoomed the camera 4x)
- Dial: a level changed elsewhere while a finger rests on the knob is followed, and the next turn starts from it (it jumped back by up to 17 dB); the wheel stops turning at the ends; NO TARGET when Drives can't be found, and Status says whether the component or the control name is wrong
- Sign-In: a save that fails isn't welcomed, counted or pulsed on Submitted, and the form stays; pressing Sign In twice no longer puts an error over the welcome; new **Clear Signature** (Redo); company and visiting are trimmed; a dot next to a stroke isn't joined to it (judged in pixels); the Sign here hint and line are dark enough to read on a panel; Status gives the reason without a script position
- Calibration takes taps only, near each target, and gives up after a minute untouched
- Zone Select: Edit Layout switches itself off after 5 minutes untouched and prints the layout
- Setup page: where the pad's top-left fits on a 1280 x 800 page; the pin is called Panel Touch
- PINS.md: every control's Lua name, and the controls without pins
- Demo: buttons act as their controls (Calibrate, Edit, Flip and Background were momentary, so they switched off on release), 44 px tabs with no title cut short (the Lobby tab is now Sign-in), Home and Redo buttons, no empty extra layer on pages 2 to 6
- Docs: Code Name in step 2, where the pad can go, the cover 2 px bigger, resizing the pasted Display, ticking both Touch Activity pins, QSC routers (inputs from 1) with a script, a TSC-101-G3's UCI is 1920x1200, the Dial's range per turn, zone-to-mute polarity, framing boxes add up, updating an older design in 10.5, Uci.SetPage in Emulate
- Test harness: the picker follows the Color Picker property; taps can arrive as two messages, the way a panel sends them

**0.9.2** (a full review, and the first run of the shipped build in Designer)
- Touch Activity: one contact can no longer fire a double tap (a single tap could send the camera home or route a source everywhere); a report that arrives after "up" no longer becomes a tap; one that beats its own "down" still lands. A Panel Touch toggle saved on with nothing wired no longer turns every tap into a hold
- A second tap before the picker parks waits for both axes (Designer sends them about 60 ms apart), so it no longer lands with the previous touch's other axis. Quick taps never add up to a long press
- Zone Select: holding a zone that was already on solos it (it used to turn everything off); a lasso that pauses isn't applied early; the layout is on the Names page and can be typed into the design
- Sign-In: without Touch Activity, a new stroke is no longer joined to the last one by a line; a failed save is reported instead of "Saved"; the day's count survives a restart and resets at midnight; the idle clear is a true 3 minutes; the CSV is formula-safe and Excel-friendly
- Drag & Drop: a drop lands at once (it waited up to 1.75 s without Touch Activity) and is taken back if the drag carries on; a second tap on a picked source puts it down; Changed pulses for routes made through the pin; Routing pins exist for displays 7 and 8
- Swipe Layer: direction and distance are judged in pixels, so long thin pads swipe the way they look
- Dial: the last value always reaches the camera; Drives accepts component names with "/" and reports a wrong name on Status; Step Up / Step Down are on the Outputs page (they're pulses, not buttons)
- Cameras: VISCA position scales per brand (AVer and Sony were off by 4 to 12 times), targets kept in range, aiming starts from where the camera really is, a plain tap moves nothing, stops sent three times, zoom-only changes send only zoom, Wide works after the zoom buttons. Q-SYS cameras: old direction released before the new one is pressed, Max speed applied, the camera's speed sliders put back, focus value kept, no more matching a "speed" control that isn't pan or tilt. New **Camera Name** property. Framing works on any pad shape (it tilted short on non-16:9 pads). Looking for cameras in a big design stays inside the Core's per-call limit
- Calibration is kept per screen type (D for Designer, P for panels), works on long thin pads, and switching it off parks the picker
- The 0.9.1 warning for two Gesture Pads on one picker is removed: a script can't see other blocks unless their Script Access is on, so it couldn't work in a normal design. Each pad needs its own picker (setup step 2)
- Setup page warns when the picker can't fit a 1280 x 800 UCI; Wide angle and Optical zoom are on Setup for every camera mode; Swipe Layer and XY Pad pins use the same Live~ names as the other modes
- Docs: setup steps reordered and the Color Picker property used, 3 stacked copies of each drawing (a panel draws a disabled control at 35%, not 50% as I thought), Touch Activity limits, one finger per screen, honest test status

**0.9.1**
- Text in the drawings is plain ASCII: accented names and symbols such as × go in as character references. A touch panel decodes the drawing as Latin-1, so they would show as garbage there
- Every control handler is guarded: an error shows on Status and the next touch still works
- Status says what's wrong: a named picker that isn't a Color Picker or can't be found, a Q-SYS camera with no position control (Framing and Dial need one), a VISCA address or port that can't be right
- New **Debug Print** property (None, Gestures, All) logs gestures, or every picker report, to the block's Debug Output
- Joystick: if Touch Activity sticks on, the joystick lets go after 30 s without movement, so a camera can't keep driving forever
- Zone Select: a saved layout with bad values can't break the pad, and a layout recalled from a snapshot shows at once. Lassos over many zones cost about a third less
- Block pages: the Names page grows to fit up to 32 zones or 12 sources, zone names no longer cover their lights, and Joystick without a camera shows Home (so its pin exists)
- QSC plugin conventions: Manufacturer and build version set, every button declares its style, and the version shows on the block's pages

**0.9.0** (after the first Core + touch panel test)
- **Upgrading a UCI built with 0.8.x:** resize and move its picker as the Setup page shows (or press Calibrate). The old layout only lined up in Designer
- Gesture timing (double tap, hold, re-landing, the start-up grace) uses `Timer.Now()` (a monotonic clock on a Core) instead of `os.clock()`. On a Core `os.clock()` is the control engine's CPU time, not real time, so taps and holds misfired there while working in Emulate
- New picker layout for touch panels: a TSC-G3 draws the picker's touch area as a square, so the old "1.25 x the pad" layout only worked in Designer. The Setup page shows the exact size and position
- A landing after a park waits for both of the picker's axes and is placed where the finger first landed even on a fast drag. Touches on the picker outside the pad are ignored
- Without Touch Activity, a drag that only paused no longer runs its lift early
- Zones, Sources or Destinations set to 1 work (they crashed the block)
- Double-tap Home moves the camera on a Core; Q-SYS cameras that report position on `ptz.preset` (NC series) aim and frame by position
- VISCA: inquiries use the inquiry message type, replies are parsed per message, UDP replies are read correctly
- Joystick with Dead zone 0 no longer outputs NaN at the exact centre; the demo camera can't get stuck moving
- Sign-In: files go to `design/` in Emulate and `media/` on a Core and never overwrite each other
- No single script call comes near the Core's limit in the test harness: long lassos and signatures are bounded, and searches through the design are split into short steps
- A block whose **Color Picker** property names its picker binds straight away
- A drawing that hasn't changed isn't sent again. New **Max Frame Rate** property (default 20)
- Dial **Drives** accepts `Component~control`
- Demo UCI: page swipe off

**0.8.9**
- New Outputs page on the block, so X, Y, Tap, Double Tap, Long Press, pan/tilt speed, sign-in results and routing results are all wireable pins (Q-SYS only offers pins for controls placed on a page)
- Drag & Drop: Source Name n (text) and Changed n (pulse) per display

**0.8.4**
- Calibrate: two taps map the pad exactly onto the picker, with a live raw readout for checking a layout
- Camera Framing: on a full frame, dragging draws a new box from your finger
- Dial: shows the camera's current zoom before anyone touches it
- The picker is never moved while a finger may still be down (that ended drags in Designer)
