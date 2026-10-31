## Step 1: Embed
## Step 2: Opening Files
## Step 3: Running Scripts
## Step 4: Exporting
## Step 5: Loading Assets
## Step 6: Plugin Mode
### addImageAndWait: robust async layer insertion
### getDocumentAsImage: returns `<img>` element
### Pattern A: Open + Export UI
### Pattern B: Template + Text Edit + Export
### Pattern C: Batch Watermark
## `app`: Application Object
| `app.echoToOE(string)` | **Photopea extension**; send string to host page (captured by `runScript`) |
| `app.showWindow("magiccut")` | **Photopea extension**; open Magic Cut panel |
| `app.showWindow("vbitmap")` | **Photopea extension**; open Vectorize Bitmap panel |
## `Document`: Document Object
| `name` | string | **R/W** | **Photopea extension**; display label (no history step) |
| `source` | string | **R/W** | **Photopea extension**; file origin URL or `"local,X,NAME"` |
| `saveToOE` | `(format)` | **Photopea extension**; send binary to host. Formats: `"png"`, `"jpg:0.8"`, `"webp:0.7"`, `"psd:true"`, `"svg:true"` |
| `clearHistory` | `()` | **Photopea extension**; clear undo history to free RAM |
## `ArtLayer`: Individual Layer
| `selected` | boolean | R | **Photopea extension**; is layer highlighted in panel |
## `LayerSet`: Group Layer
## `TextItem`: Text Layer Content
| `transform` | string | JSON array; the affine transform matrix of the text |
## `SolidColor`: Color Object
## `Selection`: Selection Object
## `BlendMode` Enum: All Values
## `ExportOptionsSaveForWeb`: Export to Filesystem
## `executeAction`: Advanced Operations