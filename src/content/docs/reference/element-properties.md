---
title: Element Properties Reference — Math On Canvas
description: Full reference of all properties for every element type — position, rotation, colors, fonts, dimensions, and more for precise diagram control.
---

# Element Properties Reference

All elements share common position and rotation properties. Additional properties depend on the element type.

## Common Properties

| Property | Description |
|---|---|
| x, y | Position on canvas |
| rotation | Rotation angle in degrees |
| id | Unique element identifier |

## Text

| Property | Values |
|---|---|
| fontFamily | Font name |
| fontSize | Size in px |
| color | Hex color |
| bold | true / false |
| italic | true / false |
| fixedWidth | Width in px or null |

## Formula

| Property | Values |
|---|---|
| latex | LaTeX string |
| fontSize | Size in px |
| fontFamily | Font name |
| color | Hex color |
| inline | true / false |

## 2D Shape (all kinds)

| Property | Values |
|---|---|
| strokeColor | Hex color |
| fillColor | Hex color |
| fillOpacity | 0–1 |
| strokeWidth | px |
| dash | solid / dashed / dotted / dash-dot |

## 3D Shape (all kinds)

| Property | Values |
|---|---|
| width / height / depth | px |
| rotationMatrix | 3×3 matrix |
| faceOpacity | 0–1 |
| vertexLabels | true / false |
| centerMarker | true / false |

## Image

| Property | Values |
|---|---|
| src | Data URL |
| width / height | px |
| aspectRatioLocked | true / false |

## Freehand Draw

| Property | Values |
|---|---|
| strokeColor | Hex color |
| strokeWidth | px |
| opacity | 0–1 |
| tool | pencil / marker |
