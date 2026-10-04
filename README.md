# Heavy Type

A typography toy where letters have weight. Type anything and each letter falls, collides and piles up. Drag them, throw them, bend them into rings and waves, and let them play notes as they hit.

Everything lives in one file, `heavytype.html`: plain HTML, CSS and JavaScript on a single canvas. There are no libraries and no build step.

## Run it

Open `heavytype.html` in any modern browser. The three fonts load from Google Fonts, and each has a system fallback if you are offline.

## How to use it

Start typing and letters drop in. A few things to know:

- **Capitals are heavy.** A capital letter is 1.5x bigger and about 2.25x heavier, so it shoves small letters aside and is hard to push around.
- **Drag and throw** any letter. It lights up in the accent colour while you hold it.
- **Move the cursor through the pile** to sweep letters away. Moving fast scatters them, and moving slowly only nudges them.
- **Space** leaves a gap, which matters when you assemble the text into a word layout.
- **Double-click** the canvas to burst everything outward from the cursor.

### Keyboard

| Key | Action |
| --- | --- |
| Any letter, number or symbol | Drop that character |
| Space | Leave a gap for assembled text |
| Backspace | Remove the last letter |
| Enter | Assemble or release the typed text |
| `` ` `` (backtick) | Open or close the console |
| Escape | Close the console |

On touch screens, tap empty space to bring up the keyboard.

## The console

Open it with the **Controls** tab at the bottom of the screen. It slides up and lifts the floor, so the pile rides up with it instead of being covered.

**Gravity knob.** Drag the dial to point gravity in any direction. It snaps to the four main directions when you get close, and clicking the centre switches gravity off (float). When the knob is focused, the arrow keys set a direction and Enter sets float. New letters always drop in from the side gravity comes from.

**Switches**

- **Orbit** pulls letters toward the centre with a slight swirl. Turning it on sets gravity to zero, and you can add a gravity direction afterwards.
- **Tilt** makes your phone the gravity. While it is on, physically shaking the phone also triggers a shake.
- **Magnet** pulls letters toward your cursor and swirls them around it.
- **Rain** keeps dropping random letters from whatever you typed last.
- **Bouncy** makes everything much springier.
- **Trails** leaves a fading smear behind moving letters.
- **Color** gives every key you press the next hue in a rotating spectrum.
- **Sound** turns the note on each impact on or off.

**Keys**

- **Assemble** lines the typed letters up as text (same as Enter).
- **Burst** blasts everything outward.
- **Shake** rattles the pile and gives the screen a jolt.
- **Clear** removes every letter.

**Select**

Each of these cycles through its options when you click it.

- **Layout:** Line, Ring or Wave. Clicking it assembles your text in that shape. Ring spins slowly, and Wave ripples.
- **Font:** Sans, Serif or Mono.
- **Voice:** Sine, Marimba, Pluck or Bell.
- **Theme:** switches between light and dark. It starts from your system setting.
- **Save image** saves the canvas as `heavy-type.png`.
- **Record** captures up to 30 seconds of video and sound as `heavy-type.webm`.

Save image and Record only appear on the published Claude page, which provides the download permission they need. When you open the file locally they stay hidden.

## How it works

- **Physics.** Every letter is a circle with a radius based on its width. The simulation runs three sub-steps per frame, with gravity, wall bounces, circle-to-circle collisions, mass (capitals are heavier) and spin. Spin fades quickly on contact so letters settle instead of rolling forever. The pile is capped at 140 letters, and the oldest one is removed first.
- **Assembling.** Each letter gets a target position and angle and eases toward it, skipping collisions while it travels. Releasing the text lets the letters go one at a time in typing order, each with its own note.
- **Sound.** Each letter maps to a note in a pentatonic scale, so any sequence of letters sounds harmonious. Capitals play an octave lower. Sounds are made with the Web Audio API and a compressor on the output.
- **Drawing.** A single `<canvas>` is redrawn every frame. Letters are drawn as text, so the three fonts work without any image assets.
- **Theming.** Colours are CSS variables on `:root`. The page follows `prefers-color-scheme`, and the Theme button sets `data-theme` on the root element to override it.

## Customize it

Most of what you might want to change sits near the top of the script or in the first few lines of the CSS.

| What | Where |
| --- | --- |
| Colours and accent | The `--bg`, `--fg`, `--acc` variables in the first CSS block, plus the dark overrides below it |
| Gravity strength | The `2000` in `setG` and in the Tilt handler |
| Bounciness | `E` in the variable list (`.32` normal, `.82` bouncy) |
| Letter size | `S` in `size()`, clamped between 56 and 130 pixels |
| Capital size and weight | `s` in `spawn()` (1.5), and `m` is `s*s` |
| Maximum letters | `bodies.length>140` in `spawn()` |
| Fonts | The `FONTS` array, plus the Google Fonts link in `<head>` |
| Notes and scales | `SC` (note offsets) and `VOICES` (sound shapes) |
| Rain speed | The `260` millisecond interval near the end of the script |

## Browser notes

- Works in current Chrome, Edge, Firefox and Safari.
- **Tilt** needs a device with motion sensors. iPhones ask for permission the first time. It assumes the phone is held upright in portrait, so directions feel rotated in landscape.
- **Record** needs a browser that supports canvas recording. Some phones, especially iPhones, may not, and `.webm` files do not open in every phone gallery.
- Sound starts after your first key press or click, because browsers block audio until you interact with the page.
- Respects reduced-motion settings for the screen jolt.

## Known limits

- Letters collide as circles, so very wide letters like W or M can overlap their neighbours slightly.
- Trails leave a faint ghost of old positions if left on for a long time. Turning them off clears the screen.
- Changing the font while letters are on screen re-measures them, which can nudge a pile out of position for a moment.

## Ideas for next steps

- A shareable link that carries the typed text.
- Letters that react to the microphone.
- A word-by-word typewriter mode that assembles each word as you finish it.
- Splitting the code into modules (physics, audio, console) for use inside a React or Vite project.
