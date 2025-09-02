# Timber-Sale-Listings
This viewer enables interactive viewing of available timber sales
# Timber Sale Areas Management Documentation

## Overview
The Timber Sale Areas Management application is an interactive web-based tool for visualizing and managing timber stand data. It displays timber sale areas on an interactive map and provides a comprehensive data table for analysis and management.

## Data Format Requirements

### File Format
- **Supported formats**: GeoJSON (.geojson, .json)
- **Coordinate system**: WGS84 (EPSG:4326) recommended
- **Geometry types**: Polygon (preferred), MultiPolygon

### GeoJSON Structure
```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": {
        // Attribute data goes here
      },
      "geometry": {
        "type": "Polygon",
        "coordinates": [...]
      }
    }
  ]
}
```

## Supported Data Attributes

### Core Attributes
The application recognizes and specially handles these primary attributes:

| Attribute | Aliases | Type | Description | Example |
|-----------|---------|------|-------------|---------|
| `id` | `fid` | String/Number | Unique identifier for the sale area | "TS001", 12345 |
| `name` | `sale_name` | String | Descriptive name of the timber stand | "North Ridge Stand" |
| `acres` | `area` | Number | Area in acres | 45.2 |
| `species` | `dominant_species` | String | Primary tree species | "Oak-Hickory" |
| `status` | - | String | Current status of the sale area | "Active" |
| `sale_date` | `harvest_date` | String | Date of sale or planned harvest | "2024-03-15" |

### Status Values and Map Colors
The `status` attribute controls the color coding on the map:

| Status | Color | Description |
|--------|-------|-------------|
| `active` | Green (#4a7c24) | Currently active timber sales |
| `planned` | Yellow (#ffc107) | Planned future sales |
| `harvested` | Gray (#6c757d) | Completed harvests |
| `sold` | Bright Green (#28a745) | Sold but not yet harvested |
| `pending` | Orange (#fd7e14) | Pending approval/documentation |

### Extended Timber Attributes
All additional attributes will be displayed in popups and the details view:

#### Volume and Pricing
- `volume` - Estimated timber volume (e.g., "1,200 MBF", "850 Cords")
- `price_per_mbf` - Price per thousand board feet (e.g., "$850")
- `price_per_cord` - Price per cord for pulpwood (e.g., "$45")
- `total_value` - Total estimated value (e.g., "$125,000")

#### Forest Management
- `forest_type` - Forest classification (e.g., "Upland Hardwood", "Lowland Conifer")
- `site_index` - Site productivity index (e.g., 65)
- `basal_area` - Basal area per acre (e.g., "120 sq ft/acre")
- `trees_per_acre` - Tree density (e.g., 180)
- `avg_dbh` - Average diameter at breast height (e.g., "14.5 inches")

#### Harvest Details
- `harvest_method` - Method of harvest (e.g., "Selective Cut", "Clear Cut", "Shelterwood")
- `equipment_access` - Equipment accessibility (e.g., "Good", "Limited", "Seasonal")
- `slope_percent` - Average slope percentage (e.g., 12)
- `soil_type` - Dominant soil classification (e.g., "Silt Loam")

#### Administrative
- `owner` - Property owner (e.g., "Smith Family Trust")
- `forester` - Managing forester (e.g., "John Doe, CF")
- `contract_number` - Contract or permit number (e.g., "HC-2024-15")
- `logging_contractor` - Assigned contractor (e.g., "ABC Logging Inc.")
- `bid_date` - Date bids are due (e.g., "2024-02-15")
- `completion_date` - Expected completion (e.g., "2024-05-30")

#### Environmental
- `wetlands` - Presence of wetlands (e.g., "Yes", "No", "Adjacent")
- `streams` - Water features (e.g., "Seasonal creek", "None")
- `wildlife_concerns` - Special wildlife considerations (e.g., "Nesting season restrictions")
- `archaeological` - Cultural/archaeological notes (e.g., "Survey complete - clear")

#### Certification and Compliance
- `certification` - Forest certification (e.g., "FSC", "SFI", "PEFC")
- `bmp_notes` - Best Management Practices notes
- `permits_required` - Required permits (e.g., "Stream crossing permit")

## Data Table Display

### Primary Columns
The data table shows these core attributes in columns:
1. **ID** - Unique identifier
2. **Name** - Sale area name
3. **Acres** - Area (formatted with commas and "ac" suffix)
4. **Species** - Primary species
5. **Status** - Current status
6. **Sale Date** - Sale or harvest date
7. **Actions** - View and Details buttons

### Additional Data Access
- All other attributes are accessible through:
  - Map popup when clicking on a polygon
  - "Details" button in the data table
  - Hover information (for key attributes)

## Statistics Dashboard

The application automatically calculates and displays:
- **Total Sale Areas** - Count of all features
- **Total Acres** - Sum of all area values
- **Average Size** - Mean area per sale unit
- **Active Areas** - Count of areas with "active" status

## Data Validation and Defaults

### Automatic Handling
- Missing `id`: Uses feature index + 1
- Missing `name`: Shows "Unnamed"
- Missing `acres`: Shows "Calculated" or "N/A"
- Missing `species`: Shows "Mixed"
- Missing `status`: Defaults to "Active"
- Missing `sale_date`: Shows "TBD"

### Data Quality Tips
1. **Consistent naming**: Use consistent attribute names across all features
2. **Date formats**: Use ISO format (YYYY-MM-DD) or common formats (MM/DD/YYYY)
3. **Numeric values**: Ensure area values are numeric for proper statistics
4. **Status consistency**: Use standard status values for proper color coding

## Sample Data Structure

```json
{
  "type": "Feature",
  "properties": {
    "id": "TS001",
    "name": "North Ridge Stand",
    "acres": 45.2,
    "species": "Oak-Hickory",
    "status": "Active",
    "sale_date": "2024-03-15",
    "volume": "1,200 MBF",
    "price_per_mbf": "$850",
    "forest_type": "Upland Hardwood",
    "harvest_method": "Selective Cut",
    "owner": "Smith Family Trust",
    "forester": "Jane Doe, CF",
    "equipment_access": "Good",
    "wetlands": "None",
    "certification": "SFI"
  },
  "geometry": {
    "type": "Polygon",
    "coordinates": [[...]]
  }
}
```

## Usage Notes

### File Upload
- Drag and drop GeoJSON files onto the upload area
- Or click "Choose File" to browse for files
- Maximum file size depends on browser memory

### Navigation
- Click polygons on map to view details and zoom
- Click table rows to zoom to corresponding map feature
- Use layer control to switch between street map and satellite view

### Data Management
- "Load Sample Data" - Creates example timber sale areas
- "Clear Data" - Removes all data and resets statistics
- All data is session-based (not permanently stored)

## Export from QGIS

To export data from QGIS for use in this application:
1. Right-click your layer → Export → Save Features As
2. Choose format: GeoJSON
3. Set CRS: EPSG:4326 - WGS 84
4. Include all desired attribute fields
5. Save with .geojson extension

## Troubleshooting

### Common Issues
- **File not loading**: Ensure it's valid GeoJSON format
- **Wrong location**: Check coordinate system (should be WGS84)
- **Missing data**: Verify attribute names match expected fields
- **Color coding not working**: Check status values match supported options

### Browser Compatibility
- Works in all modern browsers
- Requires JavaScript enabled
- Best performance in Chrome, Firefox, Safari, Edge
