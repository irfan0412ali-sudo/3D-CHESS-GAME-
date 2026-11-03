# Custom Creator Inscriptions on Wizard Chess Altar

Remove the foreign runic text (`Opt8_Altar_Golden_Runes`) surrounding the chessboard on the obsidian altar and replace it with custom-crafted golden chiseled inscriptions featuring the user's Instagram handle (`irfanali_78600`) on two sides and YouTube handle (`@IANGamerz`) on two sides, accompanied by interactive 3D clickability and HUD profile links.

---

## User Review & Critical Decisions

> [!IMPORTANT]
> The following decisions were selected during the interactive clarification phase:

- **Board Placement**:
  - **Player Side (White / South)** & **Opponent Side (Black / North)**: Instagram ID (`irfanali_78600`) with Instagram insignia.
  - **Left Side (West)** & **Right Side (East)**: YouTube Channel (`@IANGamerz`) with YouTube play emblem.
- **Visual Aesthetic**:
  - High-fidelity golden chiseled lettering styled with gothic rune flourishes and metallic filigree, preserving the dark cathedral ambiance.
- **Interactivity & Integration**:
  - Both 3D altar plaques and HUD interface badges will be clickable to open the respective creator profiles (`instagram.com/irfanali_78600` and `youtube.com/@IANGamerz`).

---

## 1. Overview & Core Concept

- **What It Does**:
  - Hides the default foreign-language runic mesh (`Opt8_Altar_Golden_Runes`) embedded in the cathedral 3D model.
  - Inlays four high-definition golden stone-relief inscription plaques directly into the obsidian altar surface around each quadrant of the 8x8 chessboard.
  - Provides seamless raycasting interaction on the 3D altar: hovering shows a golden magical aura and pointer cursor; clicking opens the channel or profile.
  - Augments the HUD with polished creator link pills so mobile and desktop users can easily view or copy handles.
- **Handles & Links**:
  - Instagram: `irfanali_78600` &rarr; `https://www.instagram.com/irfanali_78600/`
  - YouTube: `@IANGamerz` &rarr; `https://www.youtube.com/@IANGamerz`

---

## 2. User Experience & Visual Design

### Altar Layout Diagram
```
                     NORTH (Opponent / Black Side)
                     ┌───────────────────────────┐
                     │    @irfanali_78600       │
                     └───────────────────────────┘
   WEST (Left Side)                                  EAST (Right Side)
   ┌────────────────────────┐   ┌───────────────┐    ┌────────────────────────┐
   │                        │   │               │    │                        │
   │    @IANGamerz         │   │ 8x8 CHESSBOARD│    │    @IANGamerz         │
   │                        │   │               │    │                        │
   ┌────────────────────────┘   └───────────────┘    └────────────────────────┘
                     ┌───────────────────────────┐
                     │    @irfanali_78600       │
                     └───────────────────────────┘
                     SOUTH (Player / White Side)
```

### Visual Specifications
- **Typography & Emblems**:
  - Precise vector path rendering for Instagram camera glyph and YouTube play button.
  - Chiseled serif lettering with runic filigree accents in gleaming polished gold (`#f5d76e` highlight, `#d4af37` body, `#8c6d1f` depth shading).
- **Shader & Material Setup**:
  - `MeshStandardMaterial` with metalness `0.86`, roughness `0.28`, emissive warmth `0x3a2808`, and custom normal maps generated from dynamic canvas alpha relief.
  - Seamless flush integration at altar surface elevation `Y ≈ 0.7225` with zero z-fighting.
- **HUD Enhancement**:
  - Subtle top/bottom glassmorphic social pill displaying creator links with hover transitions and copy-to-clipboard feedback.

---

## 3. Key Technical Decisions & Trade-Offs

- **Mesh Replacement vs GLB Re-encoding**:
  - *Chosen Approach*: Intercept the Cathedral GLB traversal, disable `Opt8_Altar_Golden_Runes`, and generate procedural high-resolution canvas PBR planes aligned to the obsidian altar table.
  - *Why*: Eliminates large binary file bloat, guarantees pin-sharp typography with zero compression artifacts, and enables dynamic click raycasting and hover glow effects.
- **Direct 3D Raycasting**:
  - Integrated into the existing pointer interaction pipeline without disrupting chess piece selection, dragging, or camera orbit controls.

---

## 4. Technical Architecture & Data Strategy

```
┌────────────────────────────────────────────────────────┐
│                   Cathedral 3D Scene                   │
│                                                        │
│  cathedralRoot.traverse:                               │
│    child.name === 'Opt8_Altar_Golden_Runes'            │
│    ──> child.visible = false;                          │
│                                                        │
│  createAltarSocialInscriptions():                      │
│    ├── North Inscription Mesh (Instagram)              │
│    ├── South Inscription Mesh (Instagram)              │
│    ├── East Inscription Mesh  (YouTube)                │
│    └── West Inscription Mesh  (YouTube)                │
│                                                        │
│  raycaster.intersectObjects([socialMeshes]):           │
│    ├── pointerover: emissive pulse + pointer cursor    │
│    └── click: window.open(profileUrl, '_blank')        │
└────────────────────────────────────────────────────────┘
                           │
                           ▼
┌────────────────────────────────────────────────────────┐
│                    HUD React Overlay                   │
│                                                        │
│  Social Creator Pill in App.tsx:                       │
│    ├── Instagram: @irfanali_78600                      │
│    └── YouTube: @IANGamerz                             │
└────────────────────────────────────────────────────────┘
```

---

## 5. Implementation Steps

1. **Disable Existing Runes**: Modify `option8_cathedral` traversal in `src/wizardChessEngine.js` to set `child.visible = false` for `Altar_Golden_Runes`.
2. **Build Procedural Inscription Generator**:
   - Construct high-res canvas renderer for Instagram and YouTube textures with gold leaf shading, gothic glyph borders, and normal map generation.
   - Position 4 inscription planes around the 4 borders of the chessboard on the obsidian altar.
3. **Raycast & Click Handler**:
   - Add pointer interaction to trigger golden highlight and open URLs on click.
4. **HUD Social Link Badges**:
   - Add elegant, unobtrusive creator links to `src/App.tsx` and `src/styles.css`.
5. **Verification**: Run `compile_applet` and test rendering and interaction.
