# Guardian Index.html - Documentation

## Overview
The `index.html` file in the Guardian folder is an interactive **Google Maps-based dental office locator** that displays multiple dental clinics across the United States with their locations, contact information, and website links.

---

## File Structure

### 1. **HTML Structure**
```html
<div class="row pad-top-60">
  <div class="col m-12">
    <style>
      #map { width: 100%; height: 100%; }
    </style>
    <div id="map"></div>
    <!-- External Scripts -->
    <!-- Inline JavaScript -->
  </div>
</div>
```

**Key Elements:**
- **Container**: Uses a Bootstrap grid system (`row`, `col m-12`)
- **Map Container**: `<div id="map">` - renders the interactive Google Map
- **CSS**: Inline styles to make the map responsive (100% width and height)
- **Padding**: `pad-top-60` class adds top padding for spacing

---

## External Dependencies

### Google Maps API
```javascript
<script src="https://maps.google.com/maps/api/js?key=AIzaSyC_LEm3cpL4o12Fl6NSLEfKJGkx7ZEFoK0">
</script>
```
- **Purpose**: Provides Google Maps functionality
- **API Key**: Embedded in the URL
- **Note**: The API key should be protected and kept secure

### GMaps.js Library
```javascript
<script src="https://cdn.jsdelivr.net/npm/gmaps@0.4.25/gmaps.min.js"></script>
```
- **Purpose**: Simplifies Google Maps implementation with convenient wrapper functions
- **Version**: 0.4.25
- **Source**: jsDelivr CDN

---

## JavaScript Implementation

### 1. **Breakpoint Detection**
```javascript
var active_breakpoint = breakpoint();

function breakpoint() {
  let breakpoints = {
    tablet: 768,
    desktop: 1024,
    desktophd: 1200,
    widescreen: 1800,
  };
  // ... determines viewport size
}
```

**Purpose**: Detects responsive design breakpoints to adjust map zoom level
**Returns**: String - "Mobile", "Tablet", "Desktop", "Desktop HD", or "Widescreen"
**Usage**: Adjusts initial map zoom based on device type

---

### 2. **Markers Data Array**
The file contains a large array of marker objects with the following structure:

```javascript
var markers = [
  {
    title: "Dental Office Name",
    url: "https://example.com",
    lat: 33.5767264,
    lng: -86.5180795,
    address1: "123 Main Street",
    address2: "Suite 100",
    city: "City Name",
    state: "AL",
    zip: "12345",
    phone: "205-640-1717",
    tag: "unique-identifier",
    affiliate: "",
  },
  // ... more marker objects
];
```

**Field Descriptions:**
| Field | Type | Purpose |
|-------|------|---------|
| `title` | string | Dental office name |
| `url` | string | Website URL (empty if not available) |
| `lat` | number | Latitude coordinate |
| `lng` | number | Longitude coordinate |
| `address1` | string | Primary street address |
| `address2` | string | Secondary address line (Suite, Apt, etc.) |
| `city` | string | City name |
| `state` | string | State abbreviation (2-letter) |
| `zip` | string | ZIP code |
| `phone` | string | Phone number (format: XXX-XXX-XXXX) |
| `tag` | string | Unique identifier for logo image lookup |
| `affiliate` | string | Affiliate flag (empty string or 'true') |

**Coverage Areas:**
- Alabama (AL)
- Florida (FL)
- Maryland (MD)
- Michigan (MI)
- New Jersey (NJ)
- New York (NY)
- North Carolina (NC)
- Pennsylvania (PA)
- Texas (TX)
- Virginia (VA)
- Washington DC (DC)

---

### 3. **Map Initialization**
```javascript
var map = new GMaps({
  el: '#map',
  lat: 36.1627,
  lng: -86.7816,
  zoom: (active_breakpoint === "Mobile") ? 4.0 : 5.5,
  options: {
    styles: [ /* custom styling */ ]
  }
});
```

**Configuration Details:**
| Property | Value | Purpose |
|----------|-------|---------|
| `el` | '#map' | Target DOM element ID |
| `lat` | 36.1627 | Center latitude (Nashville, TN area) |
| `lng` | -86.7816 | Center longitude |
| `zoom` | 4.0 (Mobile) / 5.5 (Desktop) | Initial zoom level |

**Custom Map Styling:**
The map uses custom CSS styling to apply a branded color scheme:
- **Base Colors**: Dark blue (#054763) and teal (#90C6BE)
- **Water**: Dark navy (#0F2031)
- **Roads**: Gray/brown tones
- **Labels**: Teal and light gray
- **Parks**: Custom green styling

---

### 4. **Responsive Behavior**
```javascript
window.addEventListener("resize", function () {
  active_breakpoint = breakpoint();
  if (active_breakpoint === "Mobile") {
    map.setZoom(4.0);
  } else {
    map.setZoom(5.5);
  }
});
```

**Functionality:**
- Listens for window resize events
- Updates breakpoint classification
- Adjusts map zoom dynamically:
  - **Mobile**: Zoom level 4.0 (wider view, less detail)
  - **Desktop+**: Zoom level 5.5 (closer view, more detail)

---

### 5. **Marker Population & Info Windows**
```javascript
markers.forEach(function (marker) {
  // Process affiliate status
  if (marker.affiliate == 'true') {
    marker.affiliate = ' <span>- Affiliate</span>';
  }

  // Create website link
  if (marker.url) {
    marker.url = '<a href="...' + marker.url + '...">Visit Website</a>';
  }

  // Build HTML content
  let content = '<p class="map-logo"><img src="/images/' + marker.tag + '.png"></p>';
  content += '<p class="map-title">' + marker.title + marker.affiliate + '</p>';
  content += '<p>' + marker.address1 + '<br>';
  // ... more content building

  // Add marker to map
  map.addMarker({
    lat: marker.lat,
    lng: marker.lng,
    title: marker.title,
    icon: { /* custom marker icon */ },
    infoWindow: { content: content }
  });
});
```

**Process:**
1. **Affiliate Processing**: Converts affiliate flag to HTML text
2. **URL Processing**: Wraps URLs in clickable links
3. **Content Building**: Creates HTML info window with:
   - Office logo (from `/images/{tag}.png`)
   - Office title and affiliate status
   - Full address
   - Clickable phone number (tel: link)
   - Website link
4. **Marker Creation**: Adds marker with custom icon and info window

**Marker Icon:**
- **Source**: `/images/marker3.png`
- **Size**: 40x40 pixels
- **Anchor Point**: (20, 40) - centers at bottom of icon

---

## CSS Classes

The following CSS classes are used in the info window content:

| Class | Purpose |
|-------|---------|
| `map-logo` | Container for office logo image |
| `map-title` | Office name and affiliate status |
| `map-phone` | Phone number and link formatting |
| `map-url` | Website link container |

---

## Key Features

### ✅ **Interactive Map Display**
- Multiple zoom levels based on device type
- Custom branded color scheme
- Responsive to window resizing

### ✅ **Comprehensive Office Data**
- 100+ dental offices across 11 states
- Complete address information
- Direct phone and website links

### ✅ **Info Windows**
- Professional office information display
- Logo branding
- One-click phone dialing
- Direct website navigation

### ✅ **Mobile Optimization**
- Responsive design with adaptive zoom
- Touch-friendly interface
- Optimized for small screens

---

## Data Statistics

| Metric | Count |
|--------|-------|
| Total Dental Offices | 100+ |
| States Covered | 11 |
| Affiliate Offices | 0 (all empty) |
| Offices with Website URLs | 95+ |

**Geographic Distribution:**
- North Carolina: ~30 offices
- Texas: ~15 offices
- Florida: ~18 offices
- New York: ~12 offices
- Michigan: ~15 offices
- Virginia: ~10 offices
- Maryland: ~5 offices
- New Jersey: ~6 offices
- Alabama: ~8 offices
- Pennsylvania: ~2 offices
- Washington DC: ~1 office

---

## Important Notes

### Security Considerations
1. **API Key Exposure**: The Google Maps API key is visible in the HTML source. Consider:
   - Implementing server-side API key management
   - Using API key restrictions (HTTP referrers)
   - Rotating the key periodically

### Performance
- Large markers data array (200+ lines) increases page load
- Consider lazy-loading or pagination for future expansion
- GMaps library is 50KB+ when loaded

### Maintenance
- **Image Assets**: Ensure all logo images exist in `/images/` folder
- **Phone Format**: All numbers use XXX-XXX-XXXX format
- **Coordinates**: Verify accuracy, especially for multi-location offices

### Browser Compatibility
- Requires modern browser with Google Maps API support
- GMaps.js requires ES5+ JavaScript support
- Responsive features require CSS media query support

---

## Usage Instructions

### To Add a New Office:
1. Add new object to `markers` array with required fields
2. Ensure coordinates are accurate (latitude, longitude)
3. Create logo image: `/images/{tag}.png`
4. Follow existing URL format
5. Test info window display

### To Modify Styling:
1. Update `styles` array in GMaps initialization
2. Adjust zoom levels in breakpoint function
3. Modify CSS classes in info window HTML template

### To Change Center Location:
```javascript
// Current center: Nashville, TN area (36.1627, -86.7816)
// Update these values in GMaps initialization:
lat: 36.1627,  // Change latitude
lng: -86.7816, // Change longitude
```

---

## File Dependencies

| Resource | Type | Status |
|----------|------|--------|
| Google Maps API | External API | ✅ Required |
| GMaps.js | External Library | ✅ Required |
| `/images/{tag}.png` | Local Images | ⚠️ Must exist |
| Bootstrap (grid) | CSS Framework | ✅ Required |

---

## Revision History

- **Created**: Based on current file state
- **Last Updated**: April 2, 2026
- **Total Markers**: 100+ dental offices
- **Coverage**: 11 US States + DC

---

## Contact & Support

For modifications or updates to this file:
- Verify map displays correctly after changes
- Test on multiple devices (mobile, tablet, desktop)
- Check that all image assets are accessible
- Validate coordinates before publishing
- Keep the API key secure

---

**End of Documentation**
