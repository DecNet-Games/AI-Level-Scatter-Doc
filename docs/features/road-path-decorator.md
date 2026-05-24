---
title: Road / Path Spline Decorator
layout: default
parent: Features
nav_order: 6
---

# Road / Path Spline Decorator

The **Road / Path Spline Decorator** (`Tools -> Decnet -> Road Path Decorator`) is a waypoint-based spline placement tool. It allows you to place environmental props alongside roads, paths, and borders with offsets.

---

## 1. Setting Up Spline Waypoints

To configure a path:
1. In the tool window, click **+ Add Camera Position**. This places a waypoint in 3D space directly at the focus pivot of your SceneView camera, snapping it to the ground.
2. Add multiple waypoints to trace your pathway in the scene.
3. The editor renders the path as a thick orange polyline with spheres representing waypoints in the SceneView.
4. You can edit the coordinate vectors directly in the waypoints list or remove individual points.

---

## 2. Offset Placement & Sides

To avoid placing objects directly in the middle of a road, you can configure lateral offsets:
* **Placement Side**: Select Left, Right, Both Sides, or Alternating.
* **Offsets**: Separate sliders for **Left Offset** and **Right Offset** determine the distance (in meters) from the spline center.
* **Spacing Along Path**: Controls the frequency of spawned objects along the spline segments.
* > **Visual Preview**: The tool draws green dotted helper lines in the SceneView at the first segment to show the offset boundary prior to placing props.

---

## 3. Placement Snapping & Generation

* **Downward Ground Snap**: The decorator walks the spline segments at your specified spacing, projects the offset points, and shoots downward physics raycasts. If a surface is found, the asset is placed at that elevation.
* **Surface Alignment**: Rotates props to match the ground surface normal and applies the profile's Y surface offset.
* **Hierarchy Cleanliness**: Parents all instantiated props under a `[Road Decorator]` root. Click **Clear Decoration** to wipe them out, or press `Ctrl+Z` to undo a run.
