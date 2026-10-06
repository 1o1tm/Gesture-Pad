# Changelog

Every version of Gesture Pad, newest first. The version is the plugin's `PluginInfo.Version`;
the date is the day the release was built. The same list, without dates, is at the end of
[docs/GUIDE.md](docs/GUIDE.md), which ships with the plugin.

## 0.9.7 (2026-10-06)

A code review of 0.9.6, and quick swipes.

- A double tap the pad doesn't take is a tap. Two quick taps on two different tiles pulsed Double Tap and said DOUBLE TAP, the second tap was lost, and a real double tap right after it didn't count
- Without Touch Activity, two quick taps on tiles next to each other are two taps. They could come in as one touch and send a source to every screen, or clear a screen
- Pages hold even shares: 50 sources are 7, 7, 6, 6, 6, 6, 6, 6 (it was seven pages of 7 and one of 1)
- Long names can't run a redraw past the Core's limit: each name is measured once, up to 128 letters, and remembered. With 64 sources and 64 screens named with 120 letters, a redraw needed more than the Core allows and labels could go missing; now the costliest call takes about half
- A name that isn't UTF-8 (written in Latin-1 by another script) shows with "?" for its odd letters (it showed nothing). Chinese, Japanese, Korean and emoji count a full letter width, so they stay inside their tile
- Swipe Layer without Touch Activity: quick swipes in a row each count. A second swipe within a quarter second of the first was lost, and a quick swipe back cancelled both
- Guide: Designer 10.5 shows the faded Display too (it said only a panel did); a source without signal can still be routed; a double tap also pulses Tap for its first tap

## 0.9.6 (2026-10-06)

A stress test: eleven made-up jobs with up to 64 sources and screens, built as three designs and tried in Designer.

- Block pages: names and routing outputs go in columns of 16. With 64 sources the Names page was one column about 2,000 px tall, and Designer couldn't scroll to its last rows. When the columns come to more than four, the names get a **Sources** page and a **Destinations** page. Lists of up to 16 look as before
- Screen tiles: when what's on a screen (its icon, or the +) would touch the screen's name, it goes on a line under the name, on every tile alike (it overlapped on short, narrow screens). Tiles with room look as before
- Names are fitted to the width they really take in Roboto, the panel's font: no more cutting a name that fits, or running one past its tile. The tile under a dragged finger cuts its name to fit too (it ran up to 70 px past on a narrow pad), stays on the pad, and over the screens' arrows rides above them, so the page number stays in sight
- A double tap means two taps on the same tile. A quick tap on an arrow and then on the tile next to it sent that source to every screen, or routed a screen and cleared it again
- Without Touch Activity, a source drag that rests over empty space and then carries on still drops its source (it was lost)
- "Conference Room" and "Screen Share" are screens: only Teams, Zoom, Webex, codec and call make a destination a call
- Pages hold even shares: 13 sources are 7 + 6, 17 screens 9 + 8 (not 12 + 1, 16 + 1). On pads under 220 px tall, source tiles are still a finger's size
- The dim half of an arrow bar (no page that way) does nothing and catches no drop; NOW TAP A SCREEN goes when the pick runs out; a pin writing the same route again doesn't pulse Changed; a name of only spaces shows the default name

## 0.9.5 (2026-10-06)

Asked for on the forum: more sources and destinations.

- Drag & Drop takes up to **64 sources and 64 destinations** (it was 12 and 8)
- Sources or screens that don't all fit at a finger's size go on **pages**, with arrows on the pad under the sources and under the screens. A page shows at most 12 sources and 16 screens, so the drawing a panel redraws stays small. Layouts that already fitted look exactly as before
- A long list of sources closes up before it goes on pages; screens too small to hit get a roomier grid
- Drop a source on the screens' arrows to turn their page: the source stays picked, so a tap on a screen there routes it. Without Touch Activity, a drag that pauses on the arrows turns the page and carries on. Turning a page keeps a picked source for another 8 s
- Double-tap a source still sends it to every screen, on every page. Clear All says ALL CLEARED once; the routing outputs update only the screen that changed

## 0.9.4 (2026-10-05)

From the first tester on hardware: Core 110f, TSC-101-G3, Q-SYS 10.3.

- **Live ~ Gesture** says DRAG as soon as a touch becomes a drag (it waited for the lift), and when a hold turns into a drag (it stayed on LONG PRESS)
- Without Touch Activity, a fast double tap whose second tap lands a little way from the first is a tap and a double tap. It was read as a drag, so you saw TAP, DRAG and DOUBLE TAP
- Quick taps on neighbouring zones toggle each zone (they were read as a paint from the first one)
- New **Display** page on the block: the drawing at the pad's own size, so pasted copies need no resizing, and the colour to put under them for a solid look on a panel
- README: a short version of the setup, making it look right on a panel, the tester's CPU and memory figures

## 0.9.3 (2026-10-04)

Five made-up jobs built around it, and what they found.

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

## 0.9.2 (2026-10-03)

A full review, and the first run of the shipped build in Designer.

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

## 0.9.1 (2026-10-02)

- Text in the drawings is plain ASCII: accented names and symbols such as × go in as character references. A touch panel decodes the drawing as Latin-1, so they would show as garbage there
- Every control handler is guarded: an error shows on Status and the next touch still works
- Status says what's wrong: a named picker that isn't a Color Picker or can't be found, a Q-SYS camera with no position control (Framing and Dial need one), a VISCA address or port that can't be right
- New **Debug Print** property (None, Gestures, All) logs gestures, or every picker report, to the block's Debug Output
- Joystick: if Touch Activity sticks on, the joystick lets go after 30 s without movement, so a camera can't keep driving forever
- Zone Select: a saved layout with bad values can't break the pad, and a layout recalled from a snapshot shows at once. Lassos over many zones cost about a third less
- Block pages: the Names page grows to fit up to 32 zones or 12 sources, zone names no longer cover their lights, and Joystick without a camera shows Home (so its pin exists)
- QSC plugin conventions: Manufacturer and build version set, every button declares its style, and the version shows on the block's pages

## 0.9.0 (2026-10-02)

After the first Core + touch panel test.

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

## 0.8.9 (2026-10-01)

- New Outputs page on the block, so X, Y, Tap, Double Tap, Long Press, pan/tilt speed, sign-in results and routing results are all wireable pins (Q-SYS only offers pins for controls placed on a page)
- Drag & Drop: Source Name n (text) and Changed n (pulse) per display

## 0.8.4

- Calibrate: two taps map the pad exactly onto the picker, with a live raw readout for checking a layout
- Camera Framing: on a full frame, dragging draws a new box from your finger
- Dial: shows the camera's current zoom before anyone touches it
- The picker is never moved while a finger may still be down (that ended drags in Designer)

## Earlier versions

- **0.7.0**: Dial mode (an endless jog wheel for zoom or volume, with a dead centre and detents; drives any control or a camera's zoom); Color Picker property
- **0.6.0**: Demo (simulated) camera: a virtual PTZ with a Camera View, so the plugin can be shown and tried in emulation with no gear
- **0.5.0**: Flip Pan / Flip Tilt toggles (UCI-ready, with pins); Drag & Drop: double-tap a source to send it everywhere, sources without signal fade
- **0.4.0**: Built-in camera control: any Q-SYS camera or camera plugin (controls found by name) or VISCA over IP (AVer, Sony, PTZOptics, Lumens, Marshall, BirdDog, Avonic). The joystick drives speed, PTZ Pad aims, framing moves pan, tilt and zoom to the box. New Sign-In, Zone Select and Drag & Drop modes (Whiteboard retired). One visual style across modes
- **0.3.1** (2026-09-30): Lighter joystick, the knob waits during a pause, leaner whiteboard
- **0.3.0**: Panel Touch input (TSC Touch Activity) for exact press and release
- **0.2.0**: Long press; joystick redesign
- **0.1.x**: First build, start-up grace, stacked Display
