# Character asset provenance and license

The playable character foundation uses MakeHuman Community / MPFB core graphic assets released under **Creative Commons Zero (CC0 1.0)**. The original MPFB plugin code is a separate Blender authoring dependency; it is not bundled into the game executable.

Sources:

- MPFB source, core basemesh, macro targets, skeleton and skin weights: https://github.com/makehumancommunity/mpfb2
- MakeHuman core asset distribution: https://github.com/makehumancommunity/makehuman-assets
- License clarification: https://static.makehumancommunity.org/about/license.html
- CC0 legal text: https://creativecommons.org/publicdomain/zero/1.0/legalcode

The checked source asset headers explicitly record release to CC0 in September 2020. The named copyright holders at that release were Data Collection AB, Joel Palmius, and Jonas Hauquier. Attribution is retained here as provenance, although CC0 does not require it.

## Exact assets used

| Character component | Source asset |
|---|---|
| All six human bodies | MPFB core `base.obj`, macro targets, face targets, core `mixamo` rig and weights |
| Heller skin | `middleage_caucasian_male` and head-only blend with `old_caucasian_male`; generated stubble source described below |
| Heller hair | `short02` fitted and reshaped for revision 3; preserved revision 2 rollback uses `short04` |
| Heller wardrobe | `male_casualsuit05`, authored brown jacket, cream shirt and separate dark trousers; original UV normal/occlusion maps |
| Heller shoes | `shoes01`, with material tint |
| Rita / Velvet skin | `young_caucasian_female/young_caucasian_female.mhmat` and its diffuse texture |
| Rita / Velvet hair | `long01` |
| Rita / Velvet wardrobe | `female_elegantsuit01`, separate dark skirt and striped blouse |
| Rita / Velvet shoes | `shoes03` |
| Ox | `middleage_african_male`, `afro01`, `male_casualsuit06`, `shoes04`; authored plain work T-shirt and trousers using original normal/occlusion maps |
| Eddie | `old_caucasian_male`, `short02`, `male_casualsuit03`, `shoes01`; worn striped shirt and jeans |
| King | `middleage_african_male`, `short01`, `male_elegantsuit01`, `shoes01`; warm suit material |
| Rosa / Switch | `middleage_caucasian_female`, `ponytail01`, `male_worksuit01`, `shoes04`; fitted work overalls |
| Eyes | Core `eyes/low-poly`; Heller revision 3 onward uses fitted `eyes/high-poly` and authored hazel texture |

These are assets from the downloaded core `human` distribution, not unreviewed community clothing uploads. Their `.mhclo`, `.mhmat`, and mesh source headers were inspected for the explicit core CC0 notice. The source download and authoring runtime remain in `tools/art/downloads/human` and `tools/art/runtime/mpfb`; they are excluded from the portable player package.

## Authored modifications and animation

`tools/art/build_characters.py` defines six independent anatomy, face, skin, hair and wardrobe configurations, garment fitting, skin weighting, material factors, hidden-helper removal, accessory modeling and GLB export. The exact configurations are also written to `assets/models/characters/character_manifest.json`. Source skin package names describe the source library; the final characters use their own authored face and mixed ancestry parameters.

The editable `.blend` source files are stored in `assets/models/characters/source` and ignored by Godot's asset importer. The baseline anatomy/animation builder is `--python tools/art/build_characters.py`; optional names after `--` select characters. Heller's material revision 2 uses `--python tools/art/build_heller_visual.py`; the integrated hair/eye revision 3 uses `tools/art/staging/heller_visual_revision3/build_revision3.py` and its frozen approved revision 2 source. Both require verified copying/import of the staged GLB and blend. Rebuilding Heller with only the baseline builder would replace the visual polish.

Revolver, razor, cotton hand tape, bottle, cane and maintenance-key meshes were authored locally in Blender by this project. They are exported separately under `assets/models/characters/props` for reusable hand attachments and also attached to the common weighted rig. The revolver, razor and key are clipped or holstered in the exploration presentation; the bottle, cane and tape follow hand bones. These are mesh assets, not runtime primitive placeholders.

Idle, Walk, CrouchWalk, CrouchIdle, Interact, Attack, Fire, Hit, and Death are locally authored skeletal keyframe clips. **No Adobe Mixamo motion files or service-generated assets ship.** The core rig's name denotes bone compatibility and is not a claim that its animations came from Mixamo.

`tools/art/render_characters.py` renders the exported GLBs in Blender. `tools/art/render_survivors.gd` renders the same imported assets with Godot Forward+ in idle, walk, crouch and interaction poses. QA images under `artifacts/characters` are actual renders of these working meshes, not concept images presented as gameplay. Import/animation tests cover all six characters; rendered review remains necessary for contact, deformation and likeness.

`tools/art/test_character_grounding.py` evaluates the exported skinned geometry rather than bone metadata: it checks shoe contact at five points in each locomotion/crouch clip and body contact in the completed collapse. The result is recorded in `artifacts/characters/grounding.json`. Ground adjustment is baked into editable animation keys; it does not require a runtime rendering workaround.

The runtime `BlackoutPlayer.configure_survivor(id)` selects firearm capability and creates the appropriate hand attachment. `set_combat_equipped(bool)` switches between carried and held accessories without duplicating them. A skeleton modifier closes Heller/Rita/Rosa's grip only while their weapon is held; Ox/Eddie/King's hand poses are in the authored clips. Heller's Fire clip has a separate test against the actual imported revolver's muzzle direction.

`set_signature_item_available(bool)` removes a surrendered starting item from both presentations and removes firearm capability with it. The current model supplies measured standing/cautious collision heights; standing beneath a low ceiling is refused, and save restoration restores the matching stance. Tactical path queries sweep that same physical body after projecting Recast polygon points onto the real floor.

The final deformed-geometry measurement gives Ox a 1.988 m standing silhouette and a 1.720 m crouched silhouette. Rita measures 1.531 m and 1.320 m respectively. These are measured from the imported animated meshes, not inferred solely from macro sliders or inflated static bounds. The grounding report includes all six survivors, both stationary feet, each locomotion phase and collapsed-body contact.

These meshes remain a production foundation rather than proof that the full game's visual acceptance gate is complete. In particular, generated portrait likeness, garment wear and motion polish require comparison in the finished game scene.

## Named civilian cast

`tools/art/npc_characters.json` defines twelve additional adult identities: Dr. Miriam Bell, Celia Ortiz, Tomas Ortiz, Mara Price, Abel Stern, Officer Devlin, Silas Bellamy, Esteban Cruz, Vincent Vale, Mrs. Adelaide Price, Evelyn Shaw and Esther Wu. Each has independent body and facial targets, skin/hair choices and wardrobe material direction. These models do not carry the survivors' signature weapons.

The exact runtime names and every source selection are recorded in `assets/models/characters/npc_manifest.json`. `content/cast_appearances.json` maps authored cast IDs to `npc_*` GLB names. They use the same `assets/models/characters` directory and common skeletal/animation contract as the six survivors. Their editable sources are `source/npc_*.blend`.

Additional CC0 core assets used by these civilians are `middleage_african_female`, `old_african_male`, `old_african_female`, `old_caucasian_female`, `middleage_asian_female`, `male_casualsuit01` and `female_casualsuit01`; remaining sources are also used by the survivors above. Plain period fabrics replace the original T-shirt logos while retaining original UV normal/occlusion maps. Covered leg anatomy is removed beneath complete trousers to prevent cuff intersections. These are locally fitted mesh modifications, not new restrictions on the CC0 source assets.

Rebuild a selected civilian with `--python tools/art/build_characters.py -- npc_bell`. Both grounding and render scripts accept the same optional names after `--`. Civilian grounding results are written separately to `artifacts/characters/npc_grounding.json`, preserving survivor measurements.

## Heller material revision 2

Heller retains the same MPFB topology, skeleton, weights, nine original clips, hand/foot geometry and measured standing/cautious dimensions. The integrated GLB adds 13 locally authored 2048px maps: skin aging/stubble/brows, leather wear and roughness, cream shirt/charcoal cloth, oxblood shoes and graying temples, rasterized on the existing core UVs. Source skin, clothing AO/normal, hair and shoe textures retain their CC0 provenance. The broad jacket collar is correctly assigned leather. Facial contour changes are bounded to 1.65 mm.

`tools/art/build_heller_visual.py` is the repeatable revision 2 authoring recipe. Ignored staging includes matched GLB renders, contact checks, native comparisons and exact original rollback files. This is an improvement to the working asset, not a claim of reference-art realism.

## Heller hair and eyes revision 3

The fitted `short02` wavy hair, its diffuse/normal sources, `high-poly` eye anatomy and `brownlight_eye.png` are also MakeHuman/MPFB core CC0 assets. The high-poly eye's obsolete transparent outer shell is removed; the detailed inner globe receives a textured iris and physical corneal coat. Hair has an authored higher asymmetric fringe, sparse weighted strands, gray temples, restrained normal detail, varied roughness and a scalp-clearance fit. These are editable mesh/material changes.

`tools/art/staging/heller_visual_revision3/build_revision3.py` rebuilds from the immutable revision 2 blend. `assets/models/characters/heller_material_import.gd` preserves the source GLB clearcoat/specular extension scalars that the default importer drops, scoped to revision 3 Heller materials. There is no runtime shader or network dependency. Body, wardrobe, shoes, held prop, skeleton, all nine clips and measured collision dimensions are unchanged. The source manifest records hashes and limits, and the stage retains exact revision 2 rollback files.

Actual exported-GLB checks retain 201 passing pose/contact checks; paired native Godot images also expose the remaining fidelity gap. The face still looks too young, the fringe remains simplified, and the iris is somewhat brighter than the reference. Facial age, skin/brow detail and likeness remain unfinished; the hair/eye improvement does not close the game's visual gate.

## Heller facial age revision 4

Revision 4 extends the approved revision 3 hair/eyes with head-only facial-plane adjustments, age relief, roughness and brow/stubble detail, plus a darker, less saturated iris. The additional `old_caucasian_male/old_lightskinned_male_diffuse.png` is a MakeHuman core CC0 source, blended 22 percent with the existing middle-age skin only in the head. The inherited body/wardrobe/rig/animation sources retain the provenance above.

One project-generated stubble bitmap was created with the built-in ImageGen tool using the fictional Heller portrait as reference. This is original generated project source, not a CC0 photograph or a measured material scan. Its exact prompt, reference, output, SHA256 and processing instructions are retained in `tools/art/staging/heller_identity_revision4/sources/PROVENANCE.md`, `stubble-source-v1.prompt.txt` and the visual manifest. Broad illumination was removed before curved projection into the existing UV at a chosen 40mm repeat width. Only color detail uses the generated bitmap; roughness and normal relief are authored separately.

The repeatable builder is `tools/art/staging/heller_identity_revision4/build_revision4.py`, starting from the frozen approved revision 3 blend. The production blend contains packed textures. Maximum head displacement is 4.21mm; body below the neck, other mesh geometry, skeleton, skin weights, all nine clips, held prop and collision dimensions are unchanged. The editor-only material adapter now preserves source extension scalars for both revision 3 and revision 4 Heller materials; it is unnecessary in the shipped PCK because those scalars are baked into the processed scene.

The stage retains exact revision 3 rollback assets and 201 passing exported-GLB contact/pose checks. Twenty paired native Godot frames of the processed revision 3 and review revision 4 show the changes surviving import with no visible animation regression. This is an approved modest local improvement, not a likeness or age-gate pass: the face still reads younger than the reference, brows remain uniform, the hairline remains simplified, and skin detail is coarse at extreme close range. At gameplay distance the added detail is subtle.

