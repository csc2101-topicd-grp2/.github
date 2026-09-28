# CSC2101 Topic D — Smart City Data Collection System

An AI-powered system that uses 360° street-view imagery to automatically extract and organise urban infrastructure data, supporting traffic management, city planning, and disaster preparedness.

## Overview

This project ingests 360° street-view imagery over a client-defined area and uses a combination of OpenStreetMap data, Mapillary detections, and a Vision-Language Model (VLM) to identify and validate road and urban features, roads, signage, streetlights, fire hydrants, traffic lights, and more. Extracted data is stored with full spatial relationships and made browsable through a map-based dashboard, with the option to export findings as Excel or Shapefile datasets.

## Key Features

**Functional**
- Ingest 360° street-view images (Mapillary) over a client-defined GeoJSON area
- Run VLM-based detection to extract road and urban attributes (signage, streetlights, fire hydrants, road width)
- Store attributes with spatial relationships for querying
- Map-based dashboard for browsing extracted data
- Export reports as Excel and Shapefile

**Non-Functional**
- Local-only deployment (no auth/user roles) for the prototype stage
- Backend handles concurrent external API calls without blocking
- Data model supports spatial queries and flexible attribute schemas

## How It Works

1. **Ingest** — a client-provided GeoJSON area is segmented into road sections using OSMnx.
2. **Collect** — street-view imagery for each segment is retrieved via the Mapillary API.
3. **Preprocess** — 360° images are split into four standard views (front, back, left, right) to improve detection accuracy.
4. **Analyse** — a Vision-Language Model reads each view to detect and confirm road/urban attributes, cross-checked against OpenStreetMap and Mapillary's own detections.
5. **Store** — validated attributes and their spatial geometry are saved to a PostgreSQL/PostGIS database, linked back to their source image.
6. **Visualise & export** — results are served to a map-based dashboard and can be exported as Excel workbooks or Shapefiles.

## Architecture

- **Frontend** — server-rendered pages (HTML/CSS, Bootstrap, Jinja2) with an interactive Leaflet + OpenStreetMap map for viewing and selecting areas.
- **Backend** — FastAPI serves the API layer, coordinates a background worker for image retrieval and ML processing, and manages a PostgreSQL + PostGIS database for structured, spatially-queryable storage.
- **Machine Learning** — a VLM-based pipeline cross-references OSM and Mapillary data with visual analysis of street-view imagery to extract and validate attributes, with results scored by source agreement.

Communication between all layers happens over HTTP/REST using JSON, with GeoJSON used for geographic data interchange.

## Repositories

| Repo | Purpose |
|---|---|
| [`csc2101-topicd-frontend`](https://github.com/csc2101-topicd-grp2/csc2101-topicd-frontend) | User interface and client-side application |
| [`csc2101-topicd-backend`](https://github.com/csc2101-topicd-grp2/csc2101-topicd-backend) | API layer, database, and data ingestion pipeline |
| [`csc2101-topicd-ml`](https://github.com/csc2101-topicd-grp2/csc2101-topicd-ml) | Machine learning models for image processing and attribute extraction |
| [`csc2101-topicd-integration`](https://github.com/csc2101-topicd-grp2/csc2101-topicd-integration) | Integration layer tying the frontend, backend, and ML components together |

## Tech Stack

| Layer | Technologies |
|---|---|
| Frontend | Python, FastAPI, Jinja2, HTML/CSS, Bootstrap, JavaScript, Leaflet |
| Backend | FastAPI, PostgreSQL + PostGIS, Python background workers |
| Machine Learning | OSMnx, Mapillary SDK, OpenCV/py360convert, VLM APIs (Gemini, Qwen), GeoPandas |
| Data formats | GeoJSON, Excel (.xlsx), Shapefile (.shp) |

## Output

The system produces a two-sheet dataset:
- **Roads** — one row per road segment, with attributes, width range, source, and confidence score.
- **Features** — one row per detected object, with location, precision, facing direction, source, and confidence score.

---

*This overview reflects the project proposal and is updated as the system evolves.*
