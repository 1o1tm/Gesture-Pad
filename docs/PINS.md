# Gesture Pad pins and controls

Generated from the plugin itself. In Designer, select the block, open **Control Pins** in Properties and tick the ones you want; they appear on the block.

**Pin** is the name Designer shows (group ~ name); `n` = one per zone, source or display. **Lua name** is the control's name for a script: `local gp = Component.New("<the block's Code Name>")`, then `gp["Route 1"].Value` or `gp.SelectedList.String`. A counted control's Lua name ends in its number (`Route 1`, `Route 2`); with a count of 1 it has none (`Route`). The block's Script Access must be Script or All.


## PTZ Pad

| Pin | Lua name | Type | Dir | What it does |
|---|---|---|---|---|
| Setup ~ Pad | `Pad` | text | in | Which Color Picker is the touch surface |
| Setup ~ Refresh | `Refresh` | trigger | in | Re-scan for Color Pickers |
| Status | `Status` | status | out | Plugin status |
| Setup ~ Panel Touch | `PanelTouch` | toggle | in | Wire the TSC's Status/Control > Touch Activity here (tick the pin on both blocks first): exact press and release, and long press. A browser or the UCI Viewer still works, with its lifts guessed |
| Setup ~ Calibrate | `Calibrate` | toggle | in | Turn on, then tap the two targets on the panel (taps only; it gives up after a minute untouched) |
| Setup ~ ReleaseTime | `ReleaseTime` | number | in | Silence that counts as a lift when Panel Touch isn't wired (s) |
| Setup ~ LongPressTime | `LongPressTime` | number | in | Hold time for a long press (s) |
| Live ~ Touching | `Touching` | LED | out | On while a finger is on the pad |
| Live ~ X | `X` | number | out | Finger X, 0 = left edge, 1 = right edge (live while touching) |
| Live ~ Y | `Y` | number | out | Finger Y, 0 = bottom, 1 = top |
| Live ~ Tap | `Tap` | trigger | in/out | Pulses on a tap |
| Live ~ Double Tap | `DoubleTap` | trigger | in/out | Pulses on a double tap |
| Live ~ Long Press | `LongPress` | trigger | in/out | Pulses when a finger rests ~0.6 s (reliable with Panel Touch wired) |
| Live ~ Gesture | `Gesture` | text | out | Last gesture as text (TAP, DOUBLE TAP, LONG PRESS, DRAG, SWIPE RIGHT, ...) or a hint (NOW TAP A SCREEN, TAP THE TARGET, BOX TOO SMALL, ...); Drag & Drop shows the route it made |
| Outputs ~ Pan | `Pan` | number | out | Aim, 0..1 (PTZ Pad) |
| Outputs ~ Tilt | `Tilt` | number | out | Aim, 0..1 |
| Live ~ Pan Speed | `PanSpeed` | number | out | Pan speed, -1..1 |
| Live ~ Tilt Speed | `TiltSpeed` | number | out | Tilt speed, -1..1 |
| Outputs ~ DirLeft | `DirLeft` | LED | out | On while pushing left |
| Outputs ~ DirRight | `DirRight` | LED | out | On while pushing right |
| Outputs ~ DirUp | `DirUp` | LED | out | On while pushing up |
| Outputs ~ DirDown | `DirDown` | LED | out | On while pushing down |
| Actions ~ Home | `Home` | trigger | in/out | Camera home (Camera Framing: the room's wide shot, the box reset) |
| Setup ~ Sensitivity | `Sensitivity` | number | in | Drag-to-aim sensitivity |
| Camera ~ Flip Pan | `InvertPan` | toggle | in/out | Flip pan |
| Camera ~ Flip Tilt | `InvertTilt` | toggle | in/out | Flip tilt |
| Setup ~ Camera | `Camera` | text | in | Q-SYS camera to drive |
| Camera ~ Status | `CameraStatus` | status | out | Camera connection status |
| Camera ~ Zoom In | `ZoomIn` | momentary | in | Hold to zoom in |
| Camera ~ Zoom Out | `ZoomOut` | momentary | in | Hold to zoom out |
| Setup ~ MaxSpeed | `MaxSpeed` | number | in | Camera top speed |
| Setup ~ ZoomSpeed | `ZoomSpeed` | number | in | Zoom speed |
| Setup ~ HFOV | `HFOV` | number | in | Camera wide-angle field of view (deg), for framing and aiming |
| Setup ~ OpticalZoom | `OpticalZoom` | number | in | Camera optical zoom (x), for framing, aiming and the dial's zoom readout |

Controls without a pin (for a UCI or a script):

| Lua name | Type | What it does |
|---|---|---|
| `Display` | momentary | The pad's drawing: paste it onto the UCI 3 times over the pad |
| `Calibration` | text | Calibration result: P x0 y0 w h for panels, D ... for Designer; type it into the design to keep it |

Camera pins above are for Camera Control = Q-SYS Camera. With VISCA over IP, `Setup ~ CameraIP` (text, in) takes the place of `Setup ~ Camera`; the Demo camera has neither and adds a Camera View page. With None there are no camera pins.

## Joystick

| Pin | Lua name | Type | Dir | What it does |
|---|---|---|---|---|
| Setup ~ Pad | `Pad` | text | in | Which Color Picker is the touch surface |
| Setup ~ Refresh | `Refresh` | trigger | in | Re-scan for Color Pickers |
| Status | `Status` | status | out | Plugin status |
| Setup ~ Panel Touch | `PanelTouch` | toggle | in | Wire the TSC's Status/Control > Touch Activity here (tick the pin on both blocks first): exact press and release, and long press. A browser or the UCI Viewer still works, with its lifts guessed |
| Setup ~ Calibrate | `Calibrate` | toggle | in | Turn on, then tap the two targets on the panel (taps only; it gives up after a minute untouched) |
| Setup ~ ReleaseTime | `ReleaseTime` | number | in | Silence that counts as a lift when Panel Touch isn't wired (s) |
| Setup ~ LongPressTime | `LongPressTime` | number | in | Hold time for a long press (s) |
| Live ~ Touching | `Touching` | LED | out | On while a finger is on the pad |
| Live ~ X | `X` | number | out | Finger X, 0 = left edge, 1 = right edge (live while touching) |
| Live ~ Y | `Y` | number | out | Finger Y, 0 = bottom, 1 = top |
| Live ~ Tap | `Tap` | trigger | in/out | Pulses on a tap |
| Live ~ Double Tap | `DoubleTap` | trigger | in/out | Pulses on a double tap |
| Live ~ Long Press | `LongPress` | trigger | in/out | Pulses when a finger rests ~0.6 s (reliable with Panel Touch wired) |
| Live ~ Gesture | `Gesture` | text | out | Last gesture as text (TAP, DOUBLE TAP, LONG PRESS, DRAG, SWIPE RIGHT, ...) or a hint (NOW TAP A SCREEN, TAP THE TARGET, BOX TOO SMALL, ...); Drag & Drop shows the route it made |
| Outputs ~ JoyX | `JoyX` | number | out | Joystick X, -1..1 (springs back to 0) |
| Outputs ~ JoyY | `JoyY` | number | out | Joystick Y, -1..1 |
| Outputs ~ Magnitude | `Magnitude` | number | out | How far the stick is pushed, 0..1 |
| Outputs ~ DirLeft | `DirLeft` | LED | out | On while pushing left |
| Outputs ~ DirRight | `DirRight` | LED | out | On while pushing right |
| Outputs ~ DirUp | `DirUp` | LED | out | On while pushing up |
| Outputs ~ DirDown | `DirDown` | LED | out | On while pushing down |
| Actions ~ Home | `Home` | trigger | in/out | Camera home (Camera Framing: the room's wide shot, the box reset) |
| Setup ~ Deadzone | `Deadzone` | number | in | Joystick dead zone around centre |
| Camera ~ Flip Pan | `InvertPan` | toggle | in/out | Flip pan |
| Camera ~ Flip Tilt | `InvertTilt` | toggle | in/out | Flip tilt |
| Setup ~ Camera | `Camera` | text | in | Q-SYS camera to drive |
| Camera ~ Status | `CameraStatus` | status | out | Camera connection status |
| Camera ~ Zoom In | `ZoomIn` | momentary | in | Hold to zoom in |
| Camera ~ Zoom Out | `ZoomOut` | momentary | in | Hold to zoom out |
| Setup ~ MaxSpeed | `MaxSpeed` | number | in | Camera top speed |
| Setup ~ ZoomSpeed | `ZoomSpeed` | number | in | Zoom speed |
| Setup ~ HFOV | `HFOV` | number | in | Camera wide-angle field of view (deg), for framing and aiming |
| Setup ~ OpticalZoom | `OpticalZoom` | number | in | Camera optical zoom (x), for framing, aiming and the dial's zoom readout |

Controls without a pin (for a UCI or a script):

| Lua name | Type | What it does |
|---|---|---|
| `Display` | momentary | The pad's drawing: paste it onto the UCI 3 times over the pad |
| `Calibration` | text | Calibration result: P x0 y0 w h for panels, D ... for Designer; type it into the design to keep it |

Camera pins above are for Camera Control = Q-SYS Camera. With VISCA over IP, `Setup ~ CameraIP` (text, in) takes the place of `Setup ~ Camera`; the Demo camera has neither and adds a Camera View page. With None there are no camera pins.

## Camera Framing

| Pin | Lua name | Type | Dir | What it does |
|---|---|---|---|---|
| Setup ~ Pad | `Pad` | text | in | Which Color Picker is the touch surface |
| Setup ~ Refresh | `Refresh` | trigger | in | Re-scan for Color Pickers |
| Status | `Status` | status | out | Plugin status |
| Setup ~ Panel Touch | `PanelTouch` | toggle | in | Wire the TSC's Status/Control > Touch Activity here (tick the pin on both blocks first): exact press and release, and long press. A browser or the UCI Viewer still works, with its lifts guessed |
| Setup ~ Calibrate | `Calibrate` | toggle | in | Turn on, then tap the two targets on the panel (taps only; it gives up after a minute untouched) |
| Setup ~ ReleaseTime | `ReleaseTime` | number | in | Silence that counts as a lift when Panel Touch isn't wired (s) |
| Setup ~ LongPressTime | `LongPressTime` | number | in | Hold time for a long press (s) |
| Live ~ Touching | `Touching` | LED | out | On while a finger is on the pad |
| Live ~ X | `X` | number | out | Finger X, 0 = left edge, 1 = right edge (live while touching) |
| Live ~ Y | `Y` | number | out | Finger Y, 0 = bottom, 1 = top |
| Live ~ Tap | `Tap` | trigger | in/out | Pulses on a tap |
| Live ~ Double Tap | `DoubleTap` | trigger | in/out | Pulses on a double tap |
| Live ~ Long Press | `LongPress` | trigger | in/out | Pulses when a finger rests ~0.6 s (reliable with Panel Touch wired) |
| Live ~ Gesture | `Gesture` | text | out | Last gesture as text (TAP, DOUBLE TAP, LONG PRESS, DRAG, SWIPE RIGHT, ...) or a hint (NOW TAP A SCREEN, TAP THE TARGET, BOX TOO SMALL, ...); Drag & Drop shows the route it made |
| Outputs ~ FramePan | `FramePan` | number | out | Box centre X in the current view, 0..1: the box being drawn, then the shot sent (until the next touch) |
| Outputs ~ FrameTilt | `FrameTilt` | number | out | Box centre Y in the current view, 0..1 (up = 1) |
| Outputs ~ FrameZoom | `FrameZoom` | number | out | Box size: 0 = the whole view, 1 = the Tightest frame (not the camera's own zoom) |
| Actions ~ Apply | `Apply` | trigger | in/out | Pulses when a shot is sent (a box, or a double-tap zoom-out); Frame Pan / Tilt / Zoom hold that shot |
| Actions ~ Reset | `Reset` | trigger | in/out | Zoom all the way out where the camera points (Home is the room's shot) |
| Actions ~ Home | `Home` | trigger | in/out | Camera home (Camera Framing: the room's wide shot, the box reset) |
| Setup ~ MinFrame | `MinFrame` | number | in | Tightest shot the box can draw |
| Camera ~ Flip Pan | `InvertPan` | toggle | in/out | Flip pan |
| Camera ~ Flip Tilt | `InvertTilt` | toggle | in/out | Flip tilt |
| Setup ~ Camera | `Camera` | text | in | Q-SYS camera to drive |
| Camera ~ Status | `CameraStatus` | status | out | Camera connection status |
| Camera ~ Zoom In | `ZoomIn` | momentary | in | Hold to zoom in |
| Camera ~ Zoom Out | `ZoomOut` | momentary | in | Hold to zoom out |
| Setup ~ MaxSpeed | `MaxSpeed` | number | in | Camera top speed |
| Setup ~ ZoomSpeed | `ZoomSpeed` | number | in | Zoom speed |
| Setup ~ HFOV | `HFOV` | number | in | Camera wide-angle field of view (deg), for framing and aiming |
| Setup ~ OpticalZoom | `OpticalZoom` | number | in | Camera optical zoom (x), for framing, aiming and the dial's zoom readout |

Controls without a pin (for a UCI or a script):

| Lua name | Type | What it does |
|---|---|---|
| `Display` | momentary | The pad's drawing: paste it onto the UCI 3 times over the pad |
| `Calibration` | text | Calibration result: P x0 y0 w h for panels, D ... for Designer; type it into the design to keep it |

Camera pins above are for Camera Control = Q-SYS Camera. With VISCA over IP, `Setup ~ CameraIP` (text, in) takes the place of `Setup ~ Camera`; the Demo camera has neither and adds a Camera View page. With None there are no camera pins.

## Dial

| Pin | Lua name | Type | Dir | What it does |
|---|---|---|---|---|
| Setup ~ Pad | `Pad` | text | in | Which Color Picker is the touch surface |
| Setup ~ Refresh | `Refresh` | trigger | in | Re-scan for Color Pickers |
| Status | `Status` | status | out | Plugin status |
| Setup ~ Panel Touch | `PanelTouch` | toggle | in | Wire the TSC's Status/Control > Touch Activity here (tick the pin on both blocks first): exact press and release, and long press. A browser or the UCI Viewer still works, with its lifts guessed |
| Setup ~ Calibrate | `Calibrate` | toggle | in | Turn on, then tap the two targets on the panel (taps only; it gives up after a minute untouched) |
| Setup ~ ReleaseTime | `ReleaseTime` | number | in | Silence that counts as a lift when Panel Touch isn't wired (s) |
| Setup ~ LongPressTime | `LongPressTime` | number | in | Hold time for a long press (s) |
| Live ~ Touching | `Touching` | LED | out | On while a finger is on the pad |
| Live ~ X | `X` | number | out | Finger X, 0 = left edge, 1 = right edge (live while touching) |
| Live ~ Y | `Y` | number | out | Finger Y, 0 = bottom, 1 = top |
| Live ~ Tap | `Tap` | trigger | in/out | Pulses on a tap |
| Live ~ Double Tap | `DoubleTap` | trigger | in/out | Pulses on a double tap |
| Live ~ Long Press | `LongPress` | trigger | in/out | Pulses when a finger rests ~0.6 s (reliable with Panel Touch wired) |
| Live ~ Gesture | `Gesture` | text | out | Last gesture as text (TAP, DOUBLE TAP, LONG PRESS, DRAG, SWIPE RIGHT, ...) or a hint (NOW TAP A SCREEN, TAP THE TARGET, BOX TOO SMALL, ...); Drag & Drop shows the route it made |
| Outputs ~ DialValue | `DialValue` | number | in/out | Dial position 0..1 (in and out: wire it to anything, or set it from elsewhere) |
| Live ~ Step Up | `StepUp` | trigger | out | Pulses each detent clockwise |
| Live ~ Step Down | `StepDown` | trigger | out | Pulses each detent counter-clockwise |
| Setup ~ Turns | `Turns` | number | in | Turns of the dial for the full 0..1 range |
| Setup ~ DialTarget | `DialTarget` | text | in | Optional: a control to drive directly, as Code Name~control, e.g. Gain_1~gain (case-sensitive; that component's Script Access Script or All). A script can change it at any time |
| Setup ~ Camera | `Camera` | text | in | Q-SYS camera to drive |
| Camera ~ Status | `CameraStatus` | status | out | Camera connection status |
| Camera ~ Zoom In | `ZoomIn` | momentary | in | Hold to zoom in |
| Camera ~ Zoom Out | `ZoomOut` | momentary | in | Hold to zoom out |
| Setup ~ MaxSpeed | `MaxSpeed` | number | in | Camera top speed |
| Setup ~ ZoomSpeed | `ZoomSpeed` | number | in | Zoom speed |
| Setup ~ HFOV | `HFOV` | number | in | Camera wide-angle field of view (deg), for framing and aiming |
| Setup ~ OpticalZoom | `OpticalZoom` | number | in | Camera optical zoom (x), for framing, aiming and the dial's zoom readout |

Controls without a pin (for a UCI or a script):

| Lua name | Type | What it does |
|---|---|---|
| `Display` | momentary | The pad's drawing: paste it onto the UCI 3 times over the pad |
| `Calibration` | text | Calibration result: P x0 y0 w h for panels, D ... for Designer; type it into the design to keep it |

Camera pins above are for Camera Control = Q-SYS Camera. With VISCA over IP, `Setup ~ CameraIP` (text, in) takes the place of `Setup ~ Camera`; the Demo camera has neither and adds a Camera View page. With None there are no camera pins.

## Swipe Layer

| Pin | Lua name | Type | Dir | What it does |
|---|---|---|---|---|
| Setup ~ Pad | `Pad` | text | in | Which Color Picker is the touch surface |
| Setup ~ Refresh | `Refresh` | trigger | in | Re-scan for Color Pickers |
| Status | `Status` | status | out | Plugin status |
| Setup ~ Panel Touch | `PanelTouch` | toggle | in | Wire the TSC's Status/Control > Touch Activity here (tick the pin on both blocks first): exact press and release, and long press. A browser or the UCI Viewer still works, with its lifts guessed |
| Setup ~ Calibrate | `Calibrate` | toggle | in | Turn on, then tap the two targets on the panel (taps only; it gives up after a minute untouched) |
| Setup ~ ReleaseTime | `ReleaseTime` | number | in | Silence that counts as a lift when Panel Touch isn't wired (s) |
| Setup ~ LongPressTime | `LongPressTime` | number | in | Hold time for a long press (s) |
| Live ~ Touching | `Touching` | LED | out | On while a finger is on the pad |
| Live ~ X | `X` | number | out | Finger X, 0 = left edge, 1 = right edge (live while touching) |
| Live ~ Y | `Y` | number | out | Finger Y, 0 = bottom, 1 = top |
| Live ~ Tap | `Tap` | trigger | in/out | Pulses on a tap |
| Live ~ Double Tap | `DoubleTap` | trigger | in/out | Pulses on a double tap |
| Live ~ Long Press | `LongPress` | trigger | in/out | Pulses when a finger rests ~0.6 s (reliable with Panel Touch wired) |
| Live ~ Gesture | `Gesture` | text | out | Last gesture as text (TAP, DOUBLE TAP, LONG PRESS, DRAG, SWIPE RIGHT, ...) or a hint (NOW TAP A SCREEN, TAP THE TARGET, BOX TOO SMALL, ...); Drag & Drop shows the route it made |
| Live ~ Swipe Left | `SwipeLeft` | trigger | in/out | Pulses on a swipe left |
| Live ~ Swipe Right | `SwipeRight` | trigger | in/out | Pulses on a swipe right |
| Live ~ Swipe Up | `SwipeUp` | trigger | in/out | Pulses on a swipe up |
| Live ~ Swipe Down | `SwipeDown` | trigger | in/out | Pulses on a swipe down |
| Setup ~ SwipeDistance | `SwipeDistance` | number | in | How far a swipe must travel (fraction of the pad) |

Controls without a pin (for a UCI or a script):

| Lua name | Type | What it does |
|---|---|---|
| `Display` | momentary | The pad's drawing: paste it onto the UCI 3 times over the pad |
| `Calibration` | text | Calibration result: P x0 y0 w h for panels, D ... for Designer; type it into the design to keep it |

## XY Pad

| Pin | Lua name | Type | Dir | What it does |
|---|---|---|---|---|
| Setup ~ Pad | `Pad` | text | in | Which Color Picker is the touch surface |
| Setup ~ Refresh | `Refresh` | trigger | in | Re-scan for Color Pickers |
| Status | `Status` | status | out | Plugin status |
| Setup ~ Panel Touch | `PanelTouch` | toggle | in | Wire the TSC's Status/Control > Touch Activity here (tick the pin on both blocks first): exact press and release, and long press. A browser or the UCI Viewer still works, with its lifts guessed |
| Setup ~ Calibrate | `Calibrate` | toggle | in | Turn on, then tap the two targets on the panel (taps only; it gives up after a minute untouched) |
| Setup ~ ReleaseTime | `ReleaseTime` | number | in | Silence that counts as a lift when Panel Touch isn't wired (s) |
| Setup ~ LongPressTime | `LongPressTime` | number | in | Hold time for a long press (s) |
| Live ~ Touching | `Touching` | LED | out | On while a finger is on the pad |
| Live ~ X | `X` | number | out | Finger X, 0 = left edge, 1 = right edge (live while touching) |
| Live ~ Y | `Y` | number | out | Finger Y, 0 = bottom, 1 = top |
| Live ~ Tap | `Tap` | trigger | in/out | Pulses on a tap |
| Live ~ Double Tap | `DoubleTap` | trigger | in/out | Pulses on a double tap |
| Live ~ Long Press | `LongPress` | trigger | in/out | Pulses when a finger rests ~0.6 s (reliable with Panel Touch wired) |
| Live ~ Gesture | `Gesture` | text | out | Last gesture as text (TAP, DOUBLE TAP, LONG PRESS, DRAG, SWIPE RIGHT, ...) or a hint (NOW TAP A SCREEN, TAP THE TARGET, BOX TOO SMALL, ...); Drag & Drop shows the route it made |

Controls without a pin (for a UCI or a script):

| Lua name | Type | What it does |
|---|---|---|
| `Display` | momentary | The pad's drawing: paste it onto the UCI 3 times over the pad |
| `Calibration` | text | Calibration result: P x0 y0 w h for panels, D ... for Designer; type it into the design to keep it |

## Sign-In

| Pin | Lua name | Type | Dir | What it does |
|---|---|---|---|---|
| Setup ~ Pad | `Pad` | text | in | Which Color Picker is the touch surface |
| Setup ~ Refresh | `Refresh` | trigger | in | Re-scan for Color Pickers |
| Status | `Status` | status | out | Plugin status |
| Setup ~ Panel Touch | `PanelTouch` | toggle | in | Wire the TSC's Status/Control > Touch Activity here (tick the pin on both blocks first): exact press and release, and long press. A browser or the UCI Viewer still works, with its lifts guessed |
| Setup ~ Calibrate | `Calibrate` | toggle | in | Turn on, then tap the two targets on the panel (taps only; it gives up after a minute untouched) |
| Setup ~ ReleaseTime | `ReleaseTime` | number | in | Silence that counts as a lift when Panel Touch isn't wired (s) |
| Setup ~ LongPressTime | `LongPressTime` | number | in | Hold time for a long press (s) |
| Live ~ Touching | `Touching` | LED | out | On while a finger is on the pad |
| Live ~ X | `X` | number | out | Finger X, 0 = left edge, 1 = right edge (live while touching) |
| Live ~ Y | `Y` | number | out | Finger Y, 0 = bottom, 1 = top |
| Live ~ Tap | `Tap` | trigger | in/out | Pulses on a tap |
| Live ~ Double Tap | `DoubleTap` | trigger | in/out | Pulses on a double tap |
| Live ~ Long Press | `LongPress` | trigger | in/out | Pulses when a finger rests ~0.6 s (reliable with Panel Touch wired) |
| Live ~ Gesture | `Gesture` | text | out | Last gesture as text (TAP, DOUBLE TAP, LONG PRESS, DRAG, SWIPE RIGHT, ...) or a hint (NOW TAP A SCREEN, TAP THE TARGET, BOX TOO SMALL, ...); Drag & Drop shows the route it made |
| Guest ~ Clear | `Clear` | trigger | in | Clear the signature and the guest fields |
| Guest ~ Clear Signature | `ClearSignature` | trigger | in | Clear only the signature (what was typed stays) |
| Guest ~ Submit | `Submit` | trigger | in | Save the sign-in |
| Guest ~ Submitted | `Submitted` | trigger | out | Pulses once a sign-in is saved on the Core (not when the save fails: Status and Send Status say why) |
| Guest ~ Name | `GuestName` | text | in/out | Guest name (text) |
| Guest ~ Company | `GuestCompany` | text | in/out | Company (text) |
| Guest ~ Visiting | `Visiting` | text | in/out | Who they are visiting (text) |
| Guest ~ Last Guest | `LastGuest` | text | out | Name of the last guest |
| Guest ~ Today Count | `TodayCount` | text | out | Sign-ins today |
| Setup ~ RoomName | `RoomName` | text | in | Room name stored with each sign-in |
| Setup ~ WebhookUrl | `WebhookUrl` | text | in | Optional Teams / webhook URL |
| Setup ~ SendStatus | `SendStatus` | text | out | Result of the last webhook post |
| Setup ~ PenWidth | `PenWidth` | number | in | Signature pen width |

Controls without a pin (for a UCI or a script):

| Lua name | Type | What it does |
|---|---|---|
| `Display` | momentary | The pad's drawing: paste it onto the UCI 3 times over the pad |
| `Calibration` | text | Calibration result: P x0 y0 w h for panels, D ... for Designer; type it into the design to keep it |

## Zone Select

| Pin | Lua name | Type | Dir | What it does |
|---|---|---|---|---|
| Setup ~ Pad | `Pad` | text | in | Which Color Picker is the touch surface |
| Setup ~ Refresh | `Refresh` | trigger | in | Re-scan for Color Pickers |
| Status | `Status` | status | out | Plugin status |
| Setup ~ Panel Touch | `PanelTouch` | toggle | in | Wire the TSC's Status/Control > Touch Activity here (tick the pin on both blocks first): exact press and release, and long press. A browser or the UCI Viewer still works, with its lifts guessed |
| Setup ~ Calibrate | `Calibrate` | toggle | in | Turn on, then tap the two targets on the panel (taps only; it gives up after a minute untouched) |
| Setup ~ ReleaseTime | `ReleaseTime` | number | in | Silence that counts as a lift when Panel Touch isn't wired (s) |
| Setup ~ LongPressTime | `LongPressTime` | number | in | Hold time for a long press (s) |
| Live ~ Touching | `Touching` | LED | out | On while a finger is on the pad |
| Live ~ X | `X` | number | out | Finger X, 0 = left edge, 1 = right edge (live while touching) |
| Live ~ Y | `Y` | number | out | Finger Y, 0 = bottom, 1 = top |
| Live ~ Tap | `Tap` | trigger | in/out | Pulses on a tap |
| Live ~ Double Tap | `DoubleTap` | trigger | in/out | Pulses on a double tap |
| Live ~ Long Press | `LongPress` | trigger | in/out | Pulses when a finger rests ~0.6 s (reliable with Panel Touch wired) |
| Live ~ Gesture | `Gesture` | text | out | Last gesture as text (TAP, DOUBLE TAP, LONG PRESS, DRAG, SWIPE RIGHT, ...) or a hint (NOW TAP A SCREEN, TAP THE TARGET, BOX TOO SMALL, ...); Drag & Drop shows the route it made |
| Zones ~ Selected n | `ZoneSelected n` | toggle | in/out | Zone on/off (in and out: wire to a mute, a page station input, ...) |
| Actions ~ SelectAll | `SelectAll` | trigger | in | Select every zone |
| Actions ~ ClearAll | `ClearAll` | trigger | in | Clear every zone |
| Outputs ~ SelectedList | `SelectedList` | text | out | Selected zone names, comma separated (empty when none) |

Controls without a pin (for a UCI or a script):

| Lua name | Type | What it does |
|---|---|---|
| `Display` | momentary | The pad's drawing: paste it onto the UCI 3 times over the pad |
| `Calibration` | text | Calibration result: P x0 y0 w h for panels, D ... for Designer; type it into the design to keep it |
| `ZoneName n` | text | Zone name (text) |
| `Edit` | toggle | Edit the zone layout: drag a zone to move it, its corner to resize it, empty space to redraw the last one. Switches itself off after 5 minutes untouched and prints the layout |
| `ShowBackground` | toggle | Draw the pad's own background (off: zones over your floor-plan image) |
| `ZoneLayout` | text | The zones' positions (JSON). Type it into the design to keep a layout edited in Emulate |

## Drag & Drop

| Pin | Lua name | Type | Dir | What it does |
|---|---|---|---|---|
| Setup ~ Pad | `Pad` | text | in | Which Color Picker is the touch surface |
| Setup ~ Refresh | `Refresh` | trigger | in | Re-scan for Color Pickers |
| Status | `Status` | status | out | Plugin status |
| Setup ~ Panel Touch | `PanelTouch` | toggle | in | Wire the TSC's Status/Control > Touch Activity here (tick the pin on both blocks first): exact press and release, and long press. A browser or the UCI Viewer still works, with its lifts guessed |
| Setup ~ Calibrate | `Calibrate` | toggle | in | Turn on, then tap the two targets on the panel (taps only; it gives up after a minute untouched) |
| Setup ~ ReleaseTime | `ReleaseTime` | number | in | Silence that counts as a lift when Panel Touch isn't wired (s) |
| Setup ~ LongPressTime | `LongPressTime` | number | in | Hold time for a long press (s) |
| Live ~ Touching | `Touching` | LED | out | On while a finger is on the pad |
| Live ~ X | `X` | number | out | Finger X, 0 = left edge, 1 = right edge (live while touching) |
| Live ~ Y | `Y` | number | out | Finger Y, 0 = bottom, 1 = top |
| Live ~ Tap | `Tap` | trigger | in/out | Pulses on a tap |
| Live ~ Double Tap | `DoubleTap` | trigger | in/out | Pulses on a double tap |
| Live ~ Long Press | `LongPress` | trigger | in/out | Pulses when a finger rests ~0.6 s (reliable with Panel Touch wired) |
| Live ~ Gesture | `Gesture` | text | out | Last gesture as text (TAP, DOUBLE TAP, LONG PRESS, DRAG, SWIPE RIGHT, ...) or a hint (NOW TAP A SCREEN, TAP THE TARGET, BOX TOO SMALL, ...); Drag & Drop shows the route it made |
| Sources ~ Signal n | `SourceActive n` | toggle | in | Source has signal (wire an input's signal LED); off = dimmed |
| Routing ~ Destination n | `Route n` | integer | in/out | Source number on this display, 0 = none (in and out). QSC router selects start at 1, so don't wire it straight to one: map it in a script (README, Drag & Drop) |
| Routing ~ Changed n | `Routed n` | trigger | out | Pulses when this display's route changes (on the pad or through the pin) |
| Routing ~ Source Name n | `RouteName n` | text | out | Name of the source on this display (empty when nothing is on it) |
| Actions ~ ClearRoutes | `ClearRoutes` | trigger | in | Clear every display (and put down a tapped source) |

Controls without a pin (for a UCI or a script):

| Lua name | Type | What it does |
|---|---|---|
| `Display` | momentary | The pad's drawing: paste it onto the UCI 3 times over the pad |
| `Calibration` | text | Calibration result: P x0 y0 w h for panels, D ... for Designer; type it into the design to keep it |
| `SourceName n` | text | Source name (text) |
| `DestName n` | text | Display / destination name (text) |
