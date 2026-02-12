# Static Random Pic API Manual

This project provides a **purely static** Random Picture API. It works by generating a static site where images are renamed sequentially and a client-side JavaScript file handles the randomization logic.

## Features

- **Pure Static**: No backend or Edge Functions required. Deploy to GitHub Pages, Cloudflare Pages, Vercel, etc.
- **Random Image Generation**: Supports Horizontal (`h`) and Vertical (`v`) images.
- **Session Persistence**: Random images remain consistent during a user's session (until refresh or navigation away).
- **Swup Integration**: Automatically re-initializes when content is replaced by Swup.
- **Custom Background Logic**: Specifically optimized for a background element (`#bg-box`) with preloading and CSS variable support.
- **Static Gallery**: Generates a waterfall gallery page (`gallery.html`) displaying all images.

## Installation & Build

1.  **Prepare Images**:
    *   Place horizontal images in `ri/h/`.
    *   Place vertical images in `ri/v/`.
2.  **Install Dependencies**:
    ```bash
    npm install
    ```
    (Or just ensure you have Node.js installed; the script uses standard libraries).
3.  **Build**:
    ```bash
    node build.js
    ```

The build script will:
*   Shuffle and rename images to `1.webp`, `2.webp`, etc.
*   Output everything to the `dist/` folder.
*   Generate `dist/random.js` containing the logic and image counts.
*   Generate `dist/gallery.html` for viewing all images.

## Configuration

You can configure the domain prefix (useful for CDN usage).

### Method 1: Environment Variable (Recommended for CI/CD)
Set the `DOMAIN` environment variable before running the build.
```bash
# Example (Windows PowerShell)
$env:DOMAIN="https://your-cdn.com"; node build.js
```

### Method 2: config.json
Create a `config.json` file in the root directory:
```json
{
    "domain": "https://your-cdn.com"
}
```

*Note: Environment variables take precedence over config.json.*

## Usage

Include the generated script in your HTML:
```html
<script src="random.js"></script>
```

### 1. Random Image (`<img>`)
Use a special `alt` attribute. The script will automatically replace the `src`.
```html
<!-- For Horizontal Image -->
<img alt="random:h" title="Random Horizontal">

<!-- For Vertical Image -->
<img alt="random:v" title="Random Vertical">
```

### 2. Random Background (`#bg-box`)
The script looks for an element with `id="bg-box"`. It will:
1.  Fetch a random horizontal image.
2.  Preload it.
3.  Set it as `background-image`.
4.  Add the `.loaded` class to the element.
5.  Set CSS variables `--card-bg` and `--float-panel-bg` for transparency effects.

```html
<div id="bg-box"></div>
```

### 3. Generic Random Background (Fallback)
For other elements, you can use the `data-random-bg` attribute:
```html
<div data-random-bg="h"></div>
<div data-random-bg="v"></div>
```

## Gallery Page

A static gallery is generated at `dist/gallery.html`. It displays all processed images in a responsive waterfall layout using lazy loading.

## Development

*   **build.js**: The core build script.
*   **true.js**: The template for the client-side logic (if used as reference).
