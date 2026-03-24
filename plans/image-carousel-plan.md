# Image Carousel/Pinwheel Banner Plan

## Overview
Add a scrolling image banner at the top of the homepage to showcase science photos and fill the empty space.

## Current Images Analysis
- **Location**: `/images/pinwheel/`
- **Total**: 18 images
- **Formats**: 
  - 4 JPG files (web-ready)
  - 14 HEIC files (need conversion)

## Implementation Plan

### Step 1: Convert HEIC Images to Web-Compatible Format
HEIC files won't display in browsers. Options:
- **Option A**: Use a command-line tool like `sips` (macOS built-in) or `ImageMagick` to batch convert
- **Option B**: Manually convert using Preview app or online converter
- **Recommended**: Use `sips` command for batch conversion on macOS

```bash
# Convert all HEIC to JPG using sips
cd /Users/gnickles/Desktop/gnick18.github.io/images/pinwheel
for f in *.HEIC *.heic; do
  sips -s format jpeg "$f" --out "${f%.*}.jpg"
done
```

### Step 2: CSS Styles for Scrolling Banner
Add CSS to create a smooth infinite horizontal scroll animation:

```css
/* Banner Container */
.image-banner {
  width: 100%;
  overflow: hidden;
  background: rgba(0, 0, 0, 0.05);
  padding: 1em 0;
}

/* Scrolling Track */
.banner-track {
  display: flex;
  gap: 1em;
  animation: scroll 30s linear infinite;
}

/* Pause on hover */
.banner-track:hover {
  animation-play-state: paused;
}

/* Individual Images */
.banner-track img {
  height: 200px;
  width: auto;
  object-fit: cover;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  flex-shrink: 0;
}

/* Animation Keyframes */
@keyframes scroll {
  0% { transform: translateX(0); }
  100% { transform: translateX(-50%); }
}
```

### Step 3: HTML Structure
Add the banner at the top of the intro section in `index.html`:

```html
<!-- Science Photo Banner -->
<div class="image-banner">
  <div class="banner-track">
    <!-- Images will be duplicated for infinite scroll effect -->
    <img src="images/pinwheel/image1.jpg" alt="Science photo">
    <img src="images/pinwheel/image2.jpg" alt="Science photo">
    <!-- ... more images ... -->
    <!-- Duplicate set for seamless loop -->
    <img src="images/pinwheel/image1.jpg" alt="Science photo">
    <img src="images/pinwheel/image2.jpg" alt="Science photo">
    <!-- ... more images ... -->
  </div>
</div>
```

### Step 4: JavaScript Enhancement (Optional)
For more control, add JavaScript to:
- Dynamically load images from folder
- Handle responsive image sizes
- Add pause/play controls
- Implement touch/swipe support for mobile

### Step 5: Responsive Considerations
- On mobile: reduce image height to 120-150px
- Adjust animation speed for different screen sizes
- Consider using `srcset` for optimized image loading

## Visual Design

```
┌────────────────────────────────────────────────────────────────────┐
│  [img1] [img2] [img3] [img4] [img5] [img6] [img7] [img8]  →→→     │
│                         scrolling continuously                     │
└────────────────────────────────────────────────────────────────────┘
│                                                                    │
│                    ┌─────────┐  ┌──────────────────────┐          │
│                    │ Profile │  │  Academic Pedigree   │          │
│                    │  Image  │  │        Card          │          │
│                    └─────────┘  └──────────────────────┘          │
│                                                                    │
└────────────────────────────────────────────────────────────────────┘
```

## Files to Modify
1. `index.html` - Add banner HTML structure
2. `assets/css/main.css` or inline styles - Add carousel CSS
3. `images/pinwheel/` - Convert HEIC files to JPG

## Estimated Tasks
1. Convert 14 HEIC images to JPG format
2. Add CSS styles for the scrolling animation
3. Add HTML banner structure with all images
4. Test and adjust animation speed/spacing
5. Verify mobile responsiveness

## Questions for User
- Preferred banner height? (suggested: 150-200px)
- Animation speed preference? (suggested: 30-40 seconds for full cycle)
- Should banner pause on hover?
- Any specific order for images?
