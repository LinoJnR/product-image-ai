# Product Image AI – Bulk White Background Cleaner

## Reliability and safety updates
- Processes images sequentially instead of holding decoded full-size images for the entire batch.
- Checks image decoding and reports failures per file rather than silently skipping them.
- Supports Pillow-decodable formats; optional HEIC/HEIF support is registered when `pillow-heif` is installed.
- Animated formats are treated as still images using their first frame.
- Corrects EXIF orientation before background removal.
- Protects ZIP handling against path traversal, symlink entries, excessive file counts, and oversized uncompressed archives.
- Keeps duplicate source names distinct and prevents output JPG name collisions.
- Initializes and caches the `rembg` U2Net CPU session once per app process, with a clear model initialization error.
- Enforces 25 MB per input image, 250 MB combined/uncompressed batch, 500 ZIP entries, and 100 million pixels per image. These limits protect the host; small images are not rejected.

## Workflow
1. Upload multiple images, a ZIP of images, or both.
2. Each valid image is decoded and EXIF-oriented, then background removal is applied.
3. Original foreground RGB pixels are composited onto pure white (`#FFFFFF`).
4. Every successful result is exported as JPG under `OUTPUT/IMAGES/MAIN/`.
5. Failed files are listed individually; one failed image does not stop the remaining batch.

## Important limitations
- “Any format” means any format supported by Pillow and its installed plugins. HEIC/HEIF requires `pillow-heif`; proprietary camera formats may need additional decoders.
- The first `rembg` model initialization may need internet access to download the model. Pre-cache the model for offline deployments.
- This version does not include AI upscaling, SKU/CSV matching, PT01/PT02 assignment, or automatic marketplace compliance approval.
- Output dimensions are preserved. Small images are processed rather than rejected, but are not enlarged in this version.
- Inspect the result for reflective, transparent, white, or intricate products.

## Setup (Windows PowerShell)
```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
streamlit run app.py
```
