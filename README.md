# Infinite viewport height fix for DA live preview

Have you been facing the infinite viewport height issue in DA live preview? This project demonstrates a robust fix for the common problem where `vh` units cause endless height recalculation loops in DA's live preview iframe. Implement the fix in your project similar to what's done here and you'll have a reliable solution in place!

This repository serves as both a working example and reference implementation for resolving viewport height conflicts that occur when using CSS `vh` units with DA live preview's dynamic iframe resizing.

## Environments
- Preview: https://main--da-live-preview--asthabh23.aem.page/
- Live: https://main--da-live-preview--asthabh23.aem.live/

## DA Live Preview VH Issue

When using viewport height (`vh`) units in CSS, commonly for hero blocks set to `100vh`, you may encounter an infinite loop issue in DA's live preview mode. This occurs due to the following sequence:

1. CSS sets the hero height to `100vh` (matching the current viewport)
2. DA live preview detects this as a height change and adjusts the iframe height accordingly
3. The viewport height changes due to the iframe resize
4. CSS recalculates `100vh` to match the new viewport height
5. DA live preview detects another height change
6. The cycle repeats infinitely

### Solution

The fix is implemented in `scripts/scripts.js` (lines 158-176) and works by:

1. **Adding a CSS class to the live preview iframe body**: The `da-live-preview` class is automatically added to the `<body>` element when in DA live preview mode
2. **Using targeted CSS rules**: You can create CSS rules that specifically target elements within the live preview iframe to prevent the VH loop

**Example CSS fix:**
```css
/* Normal styling */
.hero {
  height: 100vh;
}

/* Override for DA live preview to prevent VH loop */
body.da-live-preview .hero {
  height: 500px; /* Fixed height instead of vh */
  min-height: 400px;
}
```

The implementation automatically handles cases where the body element is replaced during preview updates by using a `MutationObserver` to re-apply the class as needed.

## Usage

Copy the implementation from `scripts/scripts.js` to your project and add CSS targeting `body.da-live-preview` for any blocks using `vh` units to resolve the infinite height loop.
