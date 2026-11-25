# PROJECT HAND-OFF — Offline SVG Logo Generator

## What the project is
A single-file, 100% offline vector/logo drawing app (one HTML file: index.html).
- Runs on an 11-inch Android tablet, touch + stylus. Later to be packaged as an APK.
- No accounts, no cloud, no trackers, no external links, no dependencies.
- Owner: a non-coder founder of a heritage & culture company. Uses it for company logos, game titles, presentations, app icons.
- Owner is mathematically inclined, loves angles and precision, and hates jargon.
- Hosted for previews via GitHub Pages: https://ilostmyshipsname-design.github.io/svg-offline-logo-generator/
- GitHub repo: ilostmyshipsname-design/svg-offline-logo-generator (public, branch main).
- A planning page (layout-board2.html) also lives in the repo; it is NOT part of the app.

## What is ALREADY BUILT (current index.html — do not regress these)
- Freehand drawing with pen/finger; auto shape recognition (line / circle / polygon / open stroke) with a resample-160-points + ellipse-fit + RDP-simplify recogniser.
- Selection history (undo/redo via snapshots, "hist[]" + "hi", commit()/restore()).
- Groups ("Whole Group"), per-shape and group props: cx, cy, w, h, rot, pts (normalised 0..1), kind, poly, closed, sides, colour, stroke width, rawOpen memory.
- Corner-sides counter (±), "back to original shape" memory, flip left-right / up-down.
- Single 1000×1000 artboard in a `<g id="world">`; shapes in `<g id="shapes">`; zoom/pan via scale factor V.k incl. pinch-zoom (touch + wheel).
- GAP AGREED: artboard is a fixed square. Page setup with portrait / landscape / square presets must be added (owner flagged landscape alongside portrait).
- Autosave to localStorage key `svglogo.v1` ({ver:2,shapes,nid,col,wid}) on pagehide/visibilitychange; project name + section in `svglogo.proj`.
- Five app sections already defined in the file (SECTIONS array): Home (studio overview), Studio (the drawing canvas), Font Studio (NOT built — `ready:false` stub), Project Manager (NOT built), Import & Export (NOT built).
- Bottom nav with back/forward arrows; ☰ menu opens the same sections.
- SVG and PNG export; import of images/SVG.
- Vector points note: strokes are stored as dense point chains, NOT smooth math curves.

## LOCKED design decisions (non-negotiable)
- Pinch = zoom ONLY. No two-finger rotate, no double-tap reset, no tap-undo, NO magnifying-glass/zoom buttons. Full zoom-out re-centres the artboard.
- Undo/redo = the back/forward arrows at the bottom of the screen. Nothing else.
- NO shape gallery, NO text gallery, NO in-app gallery screen. Saved works are listed on the front page (Home).
- Leaving the app keeps work on the canvas (autosave); it moves to Home only when explicitly saved/discarded via the app.
- Tool pattern (Infinite Design / ibis Paint style): every tool gets its own small popup menu with only that tool's options; menu disappears when the tool is switched off. Bars hold opener buttons only.
- Flip and similar object controls appear on/next to a selected shape's border, not parked in a bar.

## STUDIO layout (owner's own arrangement, agreed in conversation)
Buttons around the canvas; each opens its own popup:
- ✏️ Pen → pen popup = the PEN MANAGER: list of pens, add pen, delete pen (never the last one), reset pen to original, per-pen tip settings (ends blunt/round, even vs tapered, pressure). “Texture” means line behaviour, not raster grain.
- FRONT-OF-SCREEN items (owner decided, not hidden in menus):
  - Eraser = a FLOATING button parked near the canvas, draggable to wherever the owner finds effortless; position is remembered. Tap to switch active tool between pen and eraser; tap pen to switch back. Needs palm-rejection guard (deliberate tap only, ignore drawing-hand brushes).
  - Smoothing = a floating extra effect, toggleable; exact control form (dial with number vs simple badge) STILL OPEN.
- Eraser → popup with size slider. Eraser behaviour DECIDED: partial scrape-erase (rubber removes parts of strokes by cutting the stored point chains, splitting strokes into pieces), NOT tap-to-delete-whole-shape. Fine tip for minute deletions. Works with undo. Recognised/perfect shapes become ordinary point chains once scraped (expected, acceptable).
- 🎨 Colour wheel (ring + diamond picker) → popup also holds saved colour palettes and a # hex box. Replaces today's plain colour input.
- Layers button → real layer menu (show/hide eye, lock, reorder up/down). LAYERS DO NOT EXIST YET — this is a build item (a layer/group stack; groups already have some machinery: ensureParts, rebuildGroupBounds).
- Tools menu → opener for the rest (mirror, ruler, snapping, shape math, etc.) to keep the screen clean.
- Save popup → one menu: import / export / save / save as / discard.
- CHANGED BY OWNER: undo/redo arrows move from bottom bar to the TOP (revises the old 'arrows at bottom' lock). Arrow design revisited later; blocks for now.
- Shape BORDER CONTROLS for every shape (grouped or not): delete, flip, axis (meaning to pin down), copy, move, resize, plus the corners counter. Counter number-changing needs a better mechanism (current ± is inelegant) — parked.
- Selection = dashed border + grips; WHOLE vs PART selection still to be discussed together, incl. how partial erasing interplays.
- ANCHOR DOTS (node editing) — discussed, NOT built. Proposed flow so far: tap a shape once = dashed border/grips; tap AGAIN = the shape's corners/bends light up as dots; drag a dot to reshape (only that shape moves); touch the line between dots to add a dot; long-press a dot to remove it; tap a dot flips it hard-corner/smooth bend. Recognised shapes must expose real dots (circle = quarter points, square = corners). TWO OPEN OWNER QUESTIONS: entry via double-tap (no button) vs a dots button on the border; live numbers (angle/distance) while dragging vs clean drag. Reference: Infinite Painter puts "Anchor" inside its Transform: Basic/Anchor/Distort/Warp row; Infinite Design's selected-object menu (Distort, Smooth, Reshape, Liquify, Path, Expand, Transform, Delete, Settings) is the grown-up version of these border controls.
- PINNED BY OWNER: border SHAPES — owner floated the dashed selection border being round or triangle-shaped instead of a rectangle (senses something there). Parked, revisit later. Practical fix candidate to pair with it: the dashed border ROTATES WITH the shape (leaning shape = leaning box, no floating empty corners). Square/rectangle stays the default because corners = resize grips, and it fits every shape.
- Infinite Painter tools menu noted: Create/Edit two-tab split (drawing things vs fixing things) — good shelf idea for the Tools popup, owner has not decided. "Lazy" = smoothing, 3rd app confirming smoothing lives out front. IGNORED from their menus: Clone, Warp/Distort/Liquify mushing, quilt/tile.
- PERSPECTIVE GRIDS: NOT rejected (the coder wrongly filed them as knocked back — owner corrected this; perspective is maths/angles and owner wants ideas kept). On the same waiting pile as symmetry and border shapes. Future form: faint guide lines on canvas (1-point, 2-point, isometric diamond), living in the Tools menu next to ruler/symmetry, drawing guides only, never in export.
- DEPTH BUTTON ( pencilled on the waiting pile, owner sounded out, not yet confirmed): tap a drawn shape, the app grows a solid side on it — 3D block-letter/medal look. Flat maths only: copy outline, offset by angle/depth the owner sets, connect edges; output stays SVG.
- PINNED FUTURE APP — SPIN STUDIO: a SEPARATE future app that takes the flat designs exported from this app and gives them real depth, then lets the owner turn the solid object with a finger and view it from all angles (coin/medal/badge route: flat outline in, thickness added, orbit view). Factory line agreed in chat: this flat studio → Spin Studio → later the owner's VR set. The spin app has NO drawing tools of its own; its only job is file-in, depth, turn. SOURCES THE FILES: designs are made flat in the Studio here; the 3D section is preview-only (no backwards editing into the Studio). Saving: flat master stays SVG; the 3D room (when built) saves a PNG picture of a chosen view and an STL of the solid. In this app it lands as a SECTION (Home/Studio/Font Studio/Project Manager/Import & Export/3D Spin), terms: built LAST, simple first version (one solid, turntable), firewalled so 3D maths can't break drawing.
- PINNED FUTURE APP — 2D-TO-3D FOR VR BRUSH COMMUNITY: separate app idea to convert the owner's own 2D abstract drawing files into 3D files usable by VR painting/drawing people. Owner will send an example of their abstract work so scope can be seen. Related to Spin Studio but a wider, art-facing job.
- Palm rejection: LOCKED IN (owner will use stylus; ignores drawing-hand touches).
- Pen effects (stars/chain/tree lines) = stamp-along-stroke patterns (SVG dash/repeat), per-pen setting, shares machinery with the stamps feature; skeleton-first.
- ERASER = two buttons + own menu: stationary button opens menu (thickness, opacity?, materials/types causing different cut effects, edge shapes, add/create erasers); separate DRAGGABLE eraser button parked near canvas (position remembered), tap-to-switch with pen. Skeleton first.
- Menu TYPES discussed: popup, side drawer, top sheet, bottom sheet (drag handle), floating button/dial, long-press menu, radial menu, ribbon strip.

## HOME layout
Front page lists saved works to open/edit. The gallery IS Home (no separate gallery screen).
Home screen holds, in the owner's own order: Add New, Delete, Share, Save to / Save as,
Edit a saved work, Saved works list. Possibly with a side drawer for section links.
Studio bar/panel placement: still being decided group by group on the layout board.
Section icons: Home = little house; Studio icon under discussion (a square with an S was floated).

## Prioritised feature build order (agreed triage)
1. "Finishing Four": export with true transparency + auto-crop to drawing (small even margin); mirror/symmetry drawing mode (vertical at minimum, with visible centre line); proportional stroke-width scaling when resizing shapes/groups; small-size live preview box.
2. Shape math (boolean ops): join (union), cut a hole (subtract), keep the overlap (intersect). Fits the math-inclined owner; deepest upgrade.
3. Reference drawer (SLIMMED, ~3 tabs max): maths/sacred-geometry symbols + My Saved; every item gets two buttons: "Stamp as vector" or "Place as faint tracing guide" (guide auto-hides on export).
4. CAD kit: live readout (length/radius/angle), typed exact angle/size, angle snapping 15°/30°/45°/90°, then trim/extend/offset/fillet (shared machinery with shape math).
5. Font Studio (alphabet mode): guidelines (cap height, x-height, baseline), A–Z + digits + symbols slot mapper, test-typing bar; only then consider real .ttf export.
6. Quiet utilities alongside: offset/parallel path, simplify path, export size pack (avatar/header/chat-stamp).
7. Eraser + layer system + colour wheel as described above (Studio layout).

## Explicitly REJECTED (do not add)
- Shape/text/icon galleries of pre-made art.
- Vector networks, CMYK/Pantone, DXF export, reusable components, artboards, plugin systems, UI-mockup tools inside this app, 9-tab subject factories at launch, kaomoji builder for now.
- Any zoom UI controls (magnifier, +/- buttons), double-tap reset, tap-undo.
- In-app gallery screen separate from Home.

## Known accurate facts (do not misstate)
- Font/alphabet (.ttf, .otf, .woff) export is PLANNED, NOT built. The only text-related code is the Font Studio `ready:false` stub.
- There is NO text tool, NO eraser, NO layer system, NO symmetry/mirror, NO boolean ops today.
- The old README.md describes a discarded first prototype; ignore it.
- .github/workflows/enable-pages.yml is dead weight (Pages already enabled manually) — harmless, can be removed.

## Ground rules with the owner
- NOTHING gets coded without the owner's explicit go-ahead.
- Plain English only; no jargon; short answers.
- The owner drives feature decisions (with screenshots of other apps); the coder implements exactly what is agreed.
- An owner-built drag-and-drop layout board may be delivered as text/screenshot; reorganise #bar/#bot/#menu markup to match it, plus this file's Studio layout, before implementing around it.
