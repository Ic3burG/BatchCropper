# BatchCropper

[![License](https://img.shields.io/badge/license-AGPL--3.0-blue.svg)](LICENSE)

A fast, privacy-first, **client-side** batch image cropping tool that runs entirely in your browser. No uploads, no servers, no accounts — just drop your images, set a crop region once, and download all cropped files in a ZIP.

## 🔒 Privacy First

BatchCropper is designed with your privacy as the top priority.

- **100% Client-Side**: All image processing happens directly in your browser using the Canvas API. No image data is ever uploaded to a server.
- **No Dependencies to Install**: Open `index.html` in any modern browser and start cropping immediately.
- **Your Files, Your Control**: Files never leave your device.

## Features

- 🖼️ **Batch Processing**: Load multiple images at once and apply the same crop region to all of them.
- ✂️ **Visual Crop Editor**: Drag to draw a crop selection with resizable handles and a rule-of-thirds grid overlay.
- 🔢 **Precise Numeric Input**: Enter exact pixel coordinates for pixel-perfect crops.
- 🔍 **Zoom Controls**: Zoom in/out and fit-to-screen for detailed editing.
- 📦 **ZIP Export**: Download all cropped images in a single ZIP file via JSZip.
- 🎨 **Format Selection**: Export as PNG, JPEG, or WebP.
- 🌑 **Dark Mode UI**: Clean, minimal dark interface with IBM Plex Mono/Sans typography.

## Usage

1. **Open** `index.html` in any modern browser (Chrome, Firefox, Safari, Edge).
2. **Drop** your images onto the canvas area, or click to browse.
3. **Draw** a crop region by dragging on the image preview.
4. **Adjust** using the numeric inputs (X, Y, Width, Height) for precision.
5. **Select** your desired export format (PNG, JPEG, or WebP).
6. **Download** — click "Download All Cropped" to get a ZIP of all processed images.

## How It Works

- Images are loaded as `File` objects via the File API.
- The crop region is defined in native image pixels (independent of zoom level).
- On export, each image is drawn onto an off-screen `<canvas>` clipped to the crop rect, then converted to a Blob.
- All Blobs are bundled into a ZIP using [JSZip](https://stuk.github.io/jszip/) (loaded on-demand from a CDN).

## Browser Compatibility

| Browser         | Support                          |
| --------------- | -------------------------------- |
| Chrome / Edge   | ✅ Full                          |
| Firefox         | ✅ Full                          |
| Safari          | ✅ Full                          |
| Mobile browsers | ⚠️ Limited (mouse-based crop UI) |

## Running Locally

No build step required. Simply open the file:

```bash
open index.html
# or serve with any static server:
npx serve .
```

## Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines and information about our Contributor License Agreement (CLA).

## License

This project is licensed under the **GNU Affero General Public License v3.0 or later** — see the [LICENSE](LICENSE) file for details.

### Commercial Licensing

Commercial licensing is available for enterprises and professional use cases that require alternative terms. Please contact **OJD Technical Solutions** for more information.
