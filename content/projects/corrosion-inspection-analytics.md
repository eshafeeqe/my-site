---
title: "Automated Industrial Defect Inspection and ML Pipelines"
hideMeta: true
draft: false
hiddenInList: true
---

At Abyss Solutions, I developed machine learning systems for inspecting offshore industrial assets using panoramic imagery and 3D point-cloud data. My work covered corrosion segmentation, automated defect localisation, business-driven model evaluation and scalable cloud-based ML workflows.

A key contribution was an automated pipeline that transforms image segmentation results into prioritised 3D inspection points. It combines multi-view spatial clustering, non-maximum suppression and confidence-based filtering to consolidate hundreds of thousands of raw detections into actionable defect locations. The pipeline formed the technical basis for a proposed fully automated integrity-prioritisation product.

I also developed and released an improved production corrosion-segmentation model, increasing F1 across four evaluation datasets by up to 22 percentage points. A touch-group matching metric was designed for corrosion’s middle ground between segmentation and detection, treating clusters of touching regions as units. Beyond conventional image metrics, I investigated evaluation methods that better reflect downstream defect recall and inspection coverage, helping inform model and checkpoint selection.

To support model development at scale, I migrated training and evaluation workflows from constrained on-prem infrastructure to GCP using Prefect and Ray, integrating distributed execution and MLflow experiment tracking.

Prototyped a vision-language-model pipeline that parses piping isometric drawings into graphs. Combined graphs matched 22 of 22 expert-drawn line links, at about $1.90 for 36 sheets.

Tech Stack: Python, Prefect, Ray, GCP, MLflow, NumPy, 3D point-cloud processing, DBSCAN, Docker


## Demo videos 

### Auto POI Demo
<video width="100%" style="max-height: 400px; object-fit: contain;" controls>
  <source src="https://r2.eshafeeqe.space/works/abyss/auto_poi_demo.webm" type="video/mp4">
  Your browser does not support the video tag.
</video>


### Touch Group IOU
<video width="100%" style="max-height: 400px; object-fit: contain;" controls>
  <source src="https://r2.eshafeeqe.space/works/abyss/ss_group_iou_metric.webm" type="video/mp4">
  Your browser does not support the video tag.
</video>

### Isometric tool
<video width="100%" style="max-height: 400px; object-fit: contain;" controls>
  <source src="https://r2.eshafeeqe.space/works/abyss/isometric_tool_demo.webm" type="video/mp4">
  Your browser does not support the video tag.
</video>

