# StyTrix workflows — detailed recipes

All write tools take a `projectId` and place results live on `https://www.stytrix.com/canvas/{projectId}`. Always orient with `whoami` / `list_projects` / `get_credits` first (free), and confirm before spending credits.

## 1. Brief → photorealistic concept

1. `list_projects` — pick an existing project, or `create_project { title }`.
2. `generate_concept { projectId, mode: "photorealistic", prompt, aspectRatio?, imageSize? }`
   - `prompt`: the full garment description (silhouette, fabric, color, styling, background).
3. Share the canvas link and the credit cost returned.
4. Iterate: change one variable (colorway, fabric, length) and regenerate.

**Example prompt to the user's request:** "an oversized double-breasted wool trench coat in camel, editorial studio shot, soft light, 2:3".

## 2. Restyle a reference image or sketch (true-to-sketch)

`generate_concept { projectId, mode: "true_to_sketch", prompt, referenceImageUrl }`
- Use when the user provides a sketch or photo and wants it edited/restyled. `referenceImageUrl` is required.
- `prompt` is the edit instruction (e.g., "make it a cropped bomber in metallic silver").

## 3. Capsule collection

1. `create_project { title }` (e.g., "SS26 Linen Capsule").
2. `generate_model { projectId, gender?, ethnicity?, prompt?, aspectRatio? }` — a base model/figure.
3. `generate_fabric { projectId, materialType?, patternType?, primaryColor?, presentationStyle? }` — repeat for each fabric.
4. `generate_style { projectId, prompt, presentationStyle?, garmentCategory? }` for individual garments, or
   `mix_match { projectId, prompt, referenceImageUrls }` to dress a model in supplied garment images.
5. Everything lands on one canvas — share the link so the user can compare the whole capsule.

## 4. Tech-pack flat sketch

`image_to_sketch { projectId, imageUrl }` — turns a garment photo into a clean black-and-white technical flat. Good for spec sheets.

## 5. Multi-angle product views

`multi_angle { projectId, imageUrl, views?, background?, aspectRatio? }`
- `views` e.g. `["front","back","side-left"]`; `background` e.g. `"pure-white"`.

## 6. Finishing tools

- `upscale_image { projectId, imageUrl, target? }` — higher resolution.
- `remove_background { projectId, imageUrl, subject? }` — cut out the subject.
- `split_layer { projectId, imageUrl, numLayers? }` — separate into visual layers.

## 7. Runway video (async)

1. `start_video { prompt, imageUrl?, duration?, aspectRatio?, resolution? }` → returns `requestId` (charges up front).
2. Poll `check_video { requestId }` every ~1–3 min until status is done, then share the persisted video URL.

## 8. Custom trained style (async)

1. `start_style_training { styleName, referenceImageUrls, triggerWord?, loraType?, steps? }` → returns `replicateId` (charges up front; ~10–20 reference images recommended).
2. Poll `check_style_training { replicateId }` until completed, then share the trained model.

## Credit awareness

- Read-only tools (`whoami`, `list_projects`, `get_credits`, `check_*`) are free.
- Generation tools deduct credits per use (configured per tool), only on success. The tool result reports the cost and new balance — relay it.
- If `get_credits` shows an insufficient balance for the plan, tell the user to top up rather than retrying.
