# GeoCad — Professional Land Surveying & Cadastral CAD Suite for QGIS

## 1. Overview
[![QGIS Compatibility](https://img.shields.io/badge/QGIS-3.0%20%E2%80%94%204.x-brightgreen.svg)](https://qgis.org)
[![Python Version](https://img.shields.io/badge/Python-3.9%20%7C%203.10%20%7C%203.11%20%7C%203.12-blue.svg)](https://python.org)
[![License](https://img.shields.io/badge/License-GPL%20v2%2B-orange.svg)](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html)
[![Version](https://img.shields.io/badge/Release-v1.0.0-green.svg)](metadata.txt)

**GeoCad** is an enterprise-grade Land Surveying, Cadastral Lot Plotting, and Digital Terrain Modeling (DTM) CAD Workbench for QGIS Desktop. Modernized for QGIS 3 and QGIS 4, GeoCad provides a unified, production-tested CAD workflow directly inside your GIS environment.
**GeoCad** is an enterprise-grade Land Surveying, Cadastral Lot Plotting, and Digital Terrain Modeling (DTM) CAD Workbench built natively for **QGIS 3** and **QGIS 4** (Qt5 and Qt6). It provides a complete, surveyor-grade desktop workflow directly inside your GIS environment without requiring expensive proprietary CAD software.

Whether you are plotting cadastral titles from technical descriptions, importing field points from total stations/RTK-GNSS, labeling boundaries with surveyor bearings and distances, generating cartographic coordinate grids, or modeling 3D TIN surfaces and topographic contours, **GeoCad** delivers surveyor-grade precision without requiring expensive CAD licenses.
Whether you are plotting cadastral titles from technical descriptions, importing field points from total stations/RTK-GNSS, labeling boundaries with surveyor bearings and distances, generating cartographic coordinate grids, or modeling 3D TIN surfaces and topographic contours, **GeoCad** delivers millimeter-level precision and full compatibility with local and global geodetic reference frames.

---

## 2. Key Modules & Capabilities
## Architecture Overview

```
       ┌─────────────────────────────────────────────────────────────┐
       │                    GeoCad QGIS Workbench                    │
       └──────────────────────────────┬──────────────────────────────┘
                                      │
         ┌───────────────────┬────────┴──────────┬───────────────────┐
         │                   │                   │                   │
         ▼                   ▼                   ▼                   ▼
┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
│   Lot Plotter   │ │ Point Ingestion │ │Bearing & Distant│ │   Survey Grid   │
│ Bearing/Distance│ │ NEZL / KDMS2DTM │ │ Meters & Feet   │ │ Boundary Collar │
│ Tie Line / OCR  │ │ seqline Joins   │ │ 8 Placement Pos │ │ Northing/Easting│
└─────────────────┘ └─────────────────┘ └─────────────────┘ └─────────────────┘
         │                   │                   │                   │
         └───────────────────┼───────────────────┴───────────────────┘
                             │
                  ┌──────────┴──────────┐
                  │                     │
                  ▼                     ▼
         ┌─────────────────┐   ┌─────────────────┐
         │   TIN Surface   │   │   Topographic   │
         │ Delaunay Mesh   │   │    Contours     │
         │ 3D Area / Slope │   │ smocont / ezy9  │
         └─────────────────┘   └─────────────────┘
```

### 2.1. Lot Traverse Plotter (Bearing & Distance)
- **Direct Technical Description Entry**: Input cadastral lots station by station (`From`, `To`, `Bearing`, `Distance`).
- **Multi-Source Ingestion**:
  - **Upload Image (OCR)**: Scans title images, plans, and technical descriptions with built-in OCR.
  - **Paste (Image / Text)**: Seamlessly ingests clipboard text or screenshot clips from Windows Snipping Tool.
  - **Import CSV...**: Intelligently parses cadastral CSVs, GeoCad export files, and coordinate point datasets.
- **Tie-Line Calculator**: Accurately computes Station 1 starting coordinates $(E, N)$ from a reference control monument using tie bearing and distance.
- **Closure Error & Diagnostics**: Automatically calculates Easting/Northing misclosure ($c_E, c_N$), linear misclosure distance, closure direction, and relative precision ratio ($1:N$).
- **Bowditch Adjustment (Compass Rule)**: Distributes angular and distance misclosure proportionally across traverse perimeter.
---

### 2.2. Coordinate Point Plotting (`seqline`)
- Support for P,N,E,Z,L, NEL, NEZL, and KDMS field files.
- Sequential line joining for boundary traverses.
## 1. Key Modules & Capabilities

### 2.3. Boundary Bearing & Distance Annotation
- 8 standard placement modes (LR, RL, LL, RR, BL, BR, DL, DR).
- Distance units in Meters (m) or Feet (').
- Automatic text flipping to ensure annotations are never upside down.
### 1.1. Lot Traverse Plotter (Bearing & Distance)
Constructs closed cadastral boundaries from legal title technical descriptions and traverse tables.

### 2.4. Cartographic Survey Grids
- Full grid lines or cross tic marks.
- Margin collar coordinate annotations with custom units and clearance.
- **Quadrant & Azimuth Bearing Parsing**: Parses all standard surveyor bearing representations:
  - Quadrant: `N 28°38' E`, `S 80°56' E`, `S 16°35' W`, `N 65°06' W`
  - Shorthand: `N28-38E`, `S80.56E`, `S 16 35 W`, `N28.6333E`
  - Whole Circle Azimuth: `28.6333`, `109.0667`, `196.5833`
- **Multi-Channel Ingestion**:
  - **Upload Image (OCR)**: Ingests scanned land titles, cadastral survey maps, and deeds using native Windows OCR.
  - **Paste (Image / Text)**: Directly grabs copied table text or screenshot clips from the Windows Snipping Tool (`Ctrl+V`).
  - **Import CSV...**: Intelligently auto-detects cadastral CSV files, GeoCad export tables, and coordinate point datasets.
- **Interactive OCR Review Dialog**: Verifies raw extracted text side-by-side with recognized traverse legs before committing to canvas.
- **Automated Tie Point Computation**: Computes Station 1 starting coordinates $(E_1, N_1)$ from a reference control monument (e.g., BLLM No. 1, MBM, BBM) via tie bearing and distance:
  $$E_1 = E_{\text{ref}} + d_{\text{tie}} \cdot \sin(\theta_{\text{tie}})$$
  $$N_1 = N_{\text{ref}} + d_{\text{tie}} \cdot \cos(\theta_{\text{tie}})$$
- **Traverse Closure Diagnostics**: Computes linear error of closure, misclosure bearing, and precision ratio ($1:N$):
  $$\Delta E = \sum d_i \cdot \sin(\theta_i), \quad \Delta N = \sum d_i \cdot \cos(\theta_i)$$
  $$e = \sqrt{(\Delta E)^2 + (\Delta N)^2}, \quad \text{Precision} = 1 : \left( \frac{\sum d_i}{e} \right)$$
- **Bowditch (Compass Rule) Adjustment**: Automatically balances coordinates proportionally based on leg distance relative to total perimeter.
- **Layer Output**: Automatically generates clean memory layers in your target CRS:
  - `GeoCad - Lot Boundary`: Styled closed boundary polygon with metadata.
  - `GeoCad - Lot Vertices`: Point layer with numbered station labels (`Corner 1`, `Corner 2`, ...).
  - `GeoCad - Tie Line`: Distinct dashed line connecting the control monument to Corner 1.

### 2.5. Delaunay TIN Surface Modeler
- 3D triangulation and terrain modeling.
- Surface area and slope analysis.
---

### 2.6. Topographic Contour Generator
- Regular and index contours with elevation labeling.
- Spline smoothing and hypsometric elevation tinting.
### 1.2. Field Coordinate Point Ingestion & Sequential Boundaries (`seqline`)
Plots total station, RTK-GNSS, and raw coordinate survey files directly onto the QGIS canvas.

- **Automatic Format Detection**: Instantly recognizes column count and data types:
  - `P,N,E,Z,L` (Point ID, Northing, Easting, Elevation, Label)
  - `P,N,E,L` / `P,N,E,Z` / `P,N,E`
  - `N,E,Z,L` / `N,E,L` / `N,E,Z` / `N,E`
  - `E,N,Z,L` / `P,E,N,Z,L`
  - `KDMS2DTM` ASCII Survey File Format
- **Delimiter Auto-Detection**: Supports Comma (`,`), Semicolon (`;`), Tab (`\t`), Space, or mixed whitespace.
- **Sequential Line Connection (`seqline`)**: Automatically links sequential station points with identical alphabetical prefixes (e.g. `P2` through `P11` link into a linestring, while isolated control stations like `SWG.12053C` remain standalone points).
- **QGIS 4 / Qt6 Label Placement**: Employs compliant `Qgis.LabelPlacement.AroundPoint` PalLabeling buffers preventing enum type collisions.

---

### 1.3. Boundary Bearing & Distance Annotation (Meters & Feet)
Annotates existing vector boundary lines and parcel polygons with survey bearings and segment lengths.

- **8 Standard Placement Modes**:
  | Mode | Code | Description | Typical Use Case |
  | :--- | :---: | :--- | :--- |
  | **Bearing Left / Distance Right** | `LR` | Bearing outside, distance inside | Standard Cadastral Boundary |
  | **Bearing Right / Distance Left** | `RL` | Distance outside, bearing inside | Right-of-Way / Easements |
  | **Combined on Left** | `LL` | Stacked bearing & distance on left | Dense urban boundary |
  | **Combined on Right** | `RR` | Stacked bearing & distance on right | Dense urban boundary |
  | **Bearing Only Left** | `BL` | Annotates bearing only on left | Azimuth check plans |
  | **Bearing Only Right** | `BR` | Annotates bearing only on right | Azimuth check plans |
  | **Distance Only Left** | `DL` | Annotates distance only on left | Dimensioned street plans |
  | **Distance Only Right** | `DR` | Annotates distance only on right | Dimensioned street plans |
- **Dual Units**: Annotate distances in **Meters** (`246.48 m`) or **Survey Feet** (`808.66'`).
- **Dynamic Layer Binding**: Native `QgsMapLayerComboBox` automatically filters and binds active project line and polygon layers.
- **Automatic Text Flipping**: Prevents upside-down text by ensuring all annotations read comfortably from bottom-to-top and left-to-right (angles normalized within $[ -90^\circ, +90^\circ ]$).

---

### 1.4. Cartographic Survey Grid Generator
Generates precise geodetic coordinate grids with border collar callouts for map layouts and presentation sheets.

- **Grid Styles**:
  - **Full Grid Lines (`gline`)**: Continuous intersecting grid lines across bounding extent.
  - **Cross Tic Marks (`gtic`)**: Clean crosshairs at coordinate grid intersections.
- **Collar Coordinate Callouts**: Places Easting and Northing coordinate annotations around the border with customizable text size and margin offsets.
- **One-Click Extent Capture**:
  - `Use Map Canvas Extent`: Snaps grid bounds to your current view.
  - `Use Active Layer Extent`: Snaps grid bounds to the selected vector or raster layer.

---

### 1.5. Delaunay TIN Surface Modeler & Terrain Analytics
Constructs 3D triangular surface meshes from spot heights, topographic survey points, and contour vertices.

- **Dynamic Point Layer & Elevation Binding**:
  - Automatically reads 3D point geometry (`PointZ`).
  - Dropdown dynamically populates with any numeric attribute field (`Easting`, `Northing`, `Elevation`, `Z`, `Height`).
- **3D Polyface Mesh**: Generates `PolygonZ` triangle layers containing computed triangle properties:
  - `Area_2D`: Horizontal planimetric footprint ($m^2$).
  - `Area_3D`: True sloped surface area ($m^2$).
  - `Slope_deg`: Angle of inclination ($0^\circ - 90^\circ$).
  - `Aspect_deg`: Compass azimuth of maximum down-slope direction ($0^\circ - 360^\circ$).
- **Comprehensive `tininf` Terrain Analytics**: Computes 2D planimetric area, 3D true surface area, minimum/maximum/mean elevation, elevation relief ($\Delta Z$), and slope distribution.

---

### 1.6. Topographic Contour Generator & Hypsometric Tinting
Extracts smooth topographic contour lines from TIN surface layers or point clouds.

- **Index & Intermediate Contours**:
  - Automatic classification of Major Index contours (e.g., every 5th contour with heavier line weight and elevation labels).
  - Intermediate contours for fine elevation depiction.
- **Cubic Spline Smoothing (`smocont`)**: Interpolates sharp triangle intersections into natural, cartographic curved contours.
- **6-Zone Hypsometric Color Tinting (`ezy9`)**: Applies standardized elevation color ramp styling ranging from lowlands (greens) to ridges and peaks (ochres and browns).

---

### 1.7. Native QGIS Processing Framework Integration
All core algorithms are exposed directly inside the **QGIS Processing Toolbox** under the **`geocad`** provider:

| Algorithm ID | Algorithm Title | Inputs | Outputs |
| :--- | :--- | :--- | :--- |
| `geocad:plot_traverse` | Cadastral Lot Traverse Plotter | Traverse Table, Start X/Y, Tie Monument | Boundary Polygon, Vertices, Tie Line |
| `geocad:plot_points` | Survey Coordinate Point Ingestion | ASCII CSV/TXT, Profile, Delimiter | PointZ Layer, Boundary Linestrings |
| `geocad:measure_bearing_dist` | Annotate Bearings & Distances | Line/Polygon Layer, Mode, Units | Rotated Annotation Points Layer |
| `geocad:create_grid` | Cartographic Survey Grid Generator | Extent, Intervals X/Y, Style | Grid Lines/Tics, Collar Annotations |
| `geocad:create_tin` | Delaunay TIN Surface Modeler | Point Layer, Elevation Field | PolygonZ TIN Mesh Layer |
| `geocad:create_contours` | Topographic Contour Generator | Surface Mesh / Points, Interval, Spline | LineString Contours with Levels |

---

## 2. Installation & Quick Setup

### Method 1: Install from ZIP in QGIS (Recommended)
1. Download **`GeoCad.zip`** from this repository.
2. Open **QGIS Desktop** (version 3.22 or higher, including QGIS 4.x).
3. In the menu bar, navigate to **Plugins** → **Manage and Install Plugins...**
4. Click on the **Install from ZIP** tab on the left.
5. Browse to `GeoCad.zip` and click **Install Plugin**.
6. The **GeoCad** toolbar icon and **Vector** menu entries will appear immediately.

### Method 2: Manual Directory Installation
Copy the unzipped `GeoCad` directory directly into your QGIS plugins folder:

- **Windows (QGIS 4)**:
  ```powershell
  %APPDATA%\QGIS\QGIS4\profiles\default\python\plugins\GeoCad
  ```
- **Windows (QGIS 3)**:
  ```powershell
  %APPDATA%\QGIS\QGIS3\profiles\default\python\plugins\GeoCad
  ```
- **Linux**:
  ```bash
  ~/.local/share/QGIS/QGIS3/profiles/default/python/plugins/GeoCad
  ```
- **macOS**:
  ```bash
  ~/Library/Application Support/QGIS/QGIS3/profiles/default/python/plugins/GeoCad
  ```

---

## 3. Quick-Start User Guide

### 3.1. Plotting a Cadastral Lot from a Land Title
1. Click the **GeoCad Workbench** icon on the toolbar.
2. On Tab 1 (**Lot Plotter**):
   - Enter **Lot No.** (e.g. `LOT 1`) and optional declared area.
   - Select your **Target CRS** (e.g., `EPSG:3124 - PRS92 / Philippines Zone 4` or your local UTM projection).
   - Under **Coordinate System**, enter the reference control coordinates (e.g., BLLM No. 1) along with the **Tie Bearing** and **Tie Distance** to Corner 1.
3. In the traverse table:
   - Click **Upload Image (OCR)** to upload a photo/scan of your technical description, OR
   - Copy the technical description table to your clipboard and click **Paste (Image / Text)**, OR
   - Click **Import CSV...** to load a structured survey table.
4. Review the computed **Area**, **Perimeter**, and **Closure Error** in the footer metrics.
5. Click **Compute Plot Lot**. Your boundary polygon, corner vertices, and tie-line appear in the QGIS canvas.

### 3.2. Importing Survey Points (`CRS.csv`)
1. Switch to Tab 2 (**Point Ingestion & Boundaries**).
2. Click **Browse...** and select your coordinate file (e.g., `CRS.csv`).
3. Leave **Coordinate Format** and **Delimiter** on **`Auto-detect`**.
4. Check **Sequential Connection** if you want matching station prefixes (e.g. `P2..P11`) automatically joined into a boundary line.
5. Click **Plot Points to QGIS**. Points and linestrings are plotted with elevation and station labels.

### 3.3. Annotating Boundaries with Bearings and Distances
1. Switch to Tab 3 (**Bearing & Distance (Ft / M)**).
2. The **Select Layer** dropdown automatically displays your active linework or polygon layers.
3. Choose your preferred placement mode (e.g., `LR — Bearing Left / Distance Right`) and unit (`Meters (m)` or `Feet (')`).
4. Click **Annotate Layer with Bearings & Distances**.

---

## 4. Automation via Python Console / Scripting

All GeoCad operations can be scripted via the QGIS Python Console:

```python
import processing

# 1. Ingest coordinates with sequential boundary lines
pnt_result = processing.run("geocad:plot_points", {
    'INPUT_FILE': 'C:/Users/Surface/Desktop/CRS.csv',
    'PROFILE': 0,          # Auto-detect
    'DELIMITER': 0,        # Auto-detect
    'SEQUENTIAL_LINES': True,
    'PREFIX_LENGTH': 2,
    'TARGET_CRS': 'EPSG:3124',
    'OUTPUT_POINTS': 'memory:',
    'OUTPUT_LINES': 'memory:'
})

# 2. Annotate bearings and distances on the generated boundary
ann_result = processing.run("geocad:measure_bearing_dist", {
    'INPUT': pnt_result['OUTPUT_LINES'],
    'MODE': 0,             # LR: Bearing Left / Distance Right
    'UNIT': 0,             # Meters
    'FLIP_TEXT': True,
    'OUTPUT': 'memory:'
})

# Add result to map canvas
QgsProject.instance().addMapLayer(ann_result['OUTPUT'])
```

---

## 5. Repository Structure

```
GeoCad/
├── core/
│   ├── bearing_distance_annotator.py  # Surveyor bearing/distance calculations & 8 placement modes
│   ├── bearing_parser.py              # Robust quadrant, azimuth, and DMS parser
│   ├── compat.py                      # Qt5/Qt6 & QGIS 3/4 cross-version compatibility layer
│   ├── contour_engine.py              # Cubic spline smoothing & hypsometric tinting
│   ├── grid_generator.py              # Coordinate gridlines and collar callout placement
│   ├── ocr_reader.py                  # Windows OCR bridge & multi-column cadastral table parser
│   ├── point_plotter.py               # Field coordinate ingestion & seqline algorithm
│   ├── tin_engine.py                  # Delaunay 3D triangulation & tininf terrain analytics
│   └── traverse_engine.py             # Bowditch adjustment, tie-lines, and closure diagnostics
├── processing/
│   ├── alg_create_contours.py         # Topographic contour processing algorithm
│   ├── alg_create_grid.py             # Survey grid processing algorithm
│   ├── alg_create_tin.py              # TIN surface mesh processing algorithm
│   ├── alg_measure_bearing_dist.py    # Bearing & distance annotation algorithm
│   ├── alg_plot_points.py             # Point ingestion processing algorithm
│   ├── alg_plot_traverse.py           # Lot traverse plotter processing algorithm
│   └── provider.py                    # Processing provider registry ('geocad')
├── ui/
│   ├── about_dialog.py                # About & branding dialog
│   ├── bearing_distance_widget.py     # Boundary annotation tab (QgsMapLayerComboBox)
│   ├── contour_widget.py              # Topographic contouring tab
│   ├── grid_widget.py                 # Survey grid generator tab
│   ├── lot_plotter_widget.py          # Master cadastral lot traverse plotter tab
│   ├── ocr_review_dialog.py           # OCR preview and column confirmation dialog
│   ├── point_plotter_widget.py        # Coordinate ingestion tab with format auto-detection
│   └── tin_widget.py                  # Delaunay TIN surface modeler tab
├── geocad.py                          # Main plugin controller & menu integration
├── geocad_dialog.py                   # Master Workbench tabbed dialog
├── metadata.txt                       # QGIS official plugin metadata
├── icon.png                           # Plugin toolbar icon
└── README.md                          # Comprehensive documentation
```

---

## 6. License & Credits

- **Author**: GeoCad Development Team
- **Email**: `geocad@example.com`
- **License**: GNU General Public License v2.0 or later (GPL-2.0-or-later)
- **Built for**: Surveyors, Civil Engineers, Cadastral Agencies, and GIS Specialists worldwide.
