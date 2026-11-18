# Break Format Builder

A free, static, browser-only tool for turning TV network break-format screenshots into reusable break structures and formatted program lines.

## Included features

- Drag-and-drop, file-picker, and clipboard image input
- Image-specific local reading: each selected snip is analyzed from its own pixels
- Dark-popup isolation so surrounding grids, format lists, and header cells are ignored
- Light-table reading for full-width break-format grids
- Editable break rows with validation and live output
- Local format library with search, import/export, duplicate warnings, and examples
- Program-line parser with exact schedule wording and status chips
- Auto, light, and dark themes
- Responsive layout designed for a 390 px viewport
- No backend, account, analytics, API key, image upload, or runtime CDN

## Reader behavior

The reader does not use screenshot hashes, filenames, cached results, or a catalogue of saved sample values. For every selected image it:

1. Creates a new canvas from that image.
2. Detects and crops a dark popup when one is present.
3. Locates the text lines and table columns.
4. Reads the current Format Name, Duration, and each row's Center Time, Open Time, Close Time, and Break Duration.
5. Builds the output line from the rows that were just read.

The result stays editable so the operator can compare every field with the displayed source snip before saving.

## Flat-folder deployment

Keep every supplied file together at the repository root. Do not upload only `index.html`.

- `index.html` — complete interface, parser, editor, library, and reader integration
- `tesseract.min.js` — bundled offline pixel/table reader used by this build
- `worker.min.js`, `tesseract-core-lstm.wasm.js`, `tesseract-core-simd-lstm.wasm.js` — flat-folder compatibility paths
- `eng.traineddata` — bundled English reader data
- `LICENSE-tesseract.js.txt`, `LICENSE-tesseract-core.txt`
- `README.md`, `LICENSE`, `.nojekyll`, `.gitignore`

## Open locally

You can open `index.html` directly. Choose, drop, or paste a snip; all image processing remains in the browser.

For an HTTP test matching GitHub Pages, run this command in the folder:

```bash
python3 -m http.server 8000
```

Then open <http://localhost:8000>.

## GitHub Pages

1. Extract the ZIP.
2. Replace the repository's existing files with every top-level file from the extracted folder.
3. Keep all files at the repository root; do not create asset subfolders.
4. In **Settings → Pages**, use **Deploy from a branch**, `main`, and `/ (root)`.
5. Hard-refresh the deployed page with **Ctrl+Shift+R**.

All paths are relative, so the application works under a project URL such as `https://USERNAME.github.io/REPOSITORY/`.

## Verification performed in this build

Browser tests were run in Chromium through a nested HTTP project path and through `file://` using the original source images:

- `180-8`: 8 complete rows and the expected 60-second output group
- `60-4`: 4 complete rows and the expected output
- `60-3`: the popup was isolated from the overlapping side list; 3 complete rows and the expected output
- `MG6`: Format Name `MG6`, Duration `01:00:00`, all three distinct rows, and `Network Format: MG6; 90s duration @ 0:29; 60s duration @ 0:10, 0:49`
- `MG7`: Format Name `MG7`, Duration `01:00:00`, all three rows, and its distinct expected output
- No page-level horizontal scrolling at 390 px
- 1275 px centered desktop content column at 1550 px
- JavaScript syntax and ZIP integrity

The MG6 and MG7 checks use different source pixels and confirm that one image's result is not reused for the other.
