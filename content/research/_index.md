---
title: "Research"
layout: "single"
---

{{< rawhtml >}}
<style>
.research-section {
    margin: 1rem 0;
    padding: 1rem 0;
    border-bottom: 1px solid #e1e5e9;
}

.research-section:last-child {
    border-bottom: none;
}

.section-title {
    font-size: 1.8rem;
    font-weight: bold;
    margin-bottom: 1rem;
    color: #333;
}

.publication-card {
    margin-bottom: 1.5rem;
    padding: 1rem;
    border: 1px solid #e1e5e9;
    border-radius: 8px;
    background: #fafafa;
}

.publication-title {
    font-size: 1.3rem;
    font-weight: bold;
    margin-bottom: 0.5rem;
    color: #2c3e50;
}

.publication-meta {
    color: #666;
    margin-bottom: 1rem;
    font-style: italic;
}

.publication-authors {
    font-weight: 500;
    margin-bottom: 0.5rem;
}

.publication-venue {
    color: #007acc;
    font-weight: 500;
}

.publication-description {
    margin: 1rem 0;
    line-height: 1.6;
}

.image-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1rem;
    margin: 1rem 0;
}

.image-grid img {
    width: 100%;
    height: auto;
    border-radius: 4px;
    border: 1px solid #ddd;
}

.dataset-card {
    margin-bottom: 1.5rem;
    padding: 1rem;
    border: 1px solid #e1e5e9;
    border-radius: 8px;
    background: #f8f9fa;
}

.dataset-title {
    font-size: 1.2rem;
    font-weight: bold;
    margin-bottom: 0.5rem;
    color: #2c3e50;
}

.download-link {
    display: inline-block;
    padding: 0.5rem 1rem;
    background: #007acc;
    color: white;
    text-decoration: none;
    border-radius: 4px;
    margin-top: 1rem;
}

.download-link:hover {
    background: #005a9e;
}

.type-badge {
    display: inline-block;
    padding: 0.2rem 0.6rem;
    background: #28a745;
    color: white;
    font-size: 0.8rem;
    border-radius: 12px;
    margin-right: 0.5rem;
}

.dataset-badge {
    background: #17a2b8;
}
</style>

<div class="research-section">
    <h2 class="section-title">Publications</h2>
    <h3 style="margin-bottom: 1rem; color: #666;">Conferences</h3>

    <div class="publication-card">
        <h3 class="publication-title">SKELETON DRIVEN NON-RIGID MOTION TRACKING AND 3D RECONSTRUCTION</h3>
        <div class="publication-description">
            <a href="https://eshafeeqe.github.io/homepage/dicta_website/"><img src="https://eshafeeqe.github.io/homepage/shafeeq_files/DICTA.png" alt="DICTA paper thumbnail" style="width: 200px; height: auto; float: left; margin-right: 1rem; margin-bottom: 1rem;"></a>
            <div class="publication-authors"><strong>Shafeeq Elanattil</strong>, Peyman Moghadam, Simon Denman, Sridha Sridharan, Clinton Fookes.</div>
            <div class="publication-venue">Digital Image Computing: Techniques and Applications (DICTA) 2018.</div>
            <div style="margin-top: 1rem;">
                [<a href="https://eshafeeqe.github.io/homepage/dicta_website/" target="_blank"><strong>project page</strong></a>]
                [<a href="https://arxiv.org/pdf/1810.03774.pdf" target="_blank"><strong>PDF</strong></a>]
                [<a href="https://eshafeeqe.github.io/homepage/dicta_website/DICTA_Poster.pdf" target="_blank"><strong>poster</strong></a>]
            </div>
        </div>
    </div>

    <div class="publication-card">
        <h3 class="publication-title">NON-RIGID RECONSTRUCTION WITH A SINGLE MOVING RGB-D CAMERA</h3>
        <div class="publication-description">
            <a href="https://eshafeeqe.github.io/homepage"><img src="https://eshafeeqe.github.io/homepage/shafeeq_files/ICPR.png" alt="ICPR paper thumbnail" style="width: 200px; height: auto; float: left; margin-right: 1rem; margin-bottom: 1rem;"></a>
            <div class="publication-authors"><strong>Shafeeq Elanattil</strong>, Peyman Moghadam, Simon Denman, Sridha Sridharan, Clinton Fookes, Mark Cox.</div>
            <div class="publication-venue">International Conference on Pattern Recognition (ICPR) 2018.</div>
            <div style="margin-top: 1rem;">
                [<a href="https://eshafeeqe.github.io/homepage/icpr_website/" target="_blank"><strong>Project Page</strong></a>]
                [<a href="https://arxiv.org/pdf/1805.11219.pdf" target="_blank"><strong>PDF</strong></a>]
                [<a href="https://youtu.be/jkJCyTn0me0" target="_blank"><strong>Video</strong></a>]
            </div>
        </div>
    </div>

    <div class="publication-card">
        <h3 class="publication-title">NEW FEATURE DETECTION MECHANISM FOR EXTENDED KALMAN FILTER BASED MONOCULAR SLAM WITH 1-POINT RANSAC</h3>
        <div class="publication-description">
            <a href="https://arxiv.org/pdf/1805.12443.pdf"><img src="https://eshafeeqe.github.io/homepage/shafeeq_files/mike_image.png" alt="MIKE paper thumbnail" style="width: 200px; height: auto; float: left; margin-right: 1rem; margin-bottom: 1rem;"></a>
            <div class="publication-authors"><strong>Shafeeq Elanattil</strong>, Agniva Sengupta.</div>
            <div class="publication-venue">Mining Intelligence and Knowledge Exploration (MIKE) 2015.</div>
            <div style="margin-top: 1rem;">
                [<a href="https://arxiv.org/pdf/1805.12443.pdf" target="_blank"><strong>PDF</strong></a>]
            </div>
        </div>
    </div>
</div>

<div class="research-section">
    <h2 class="section-title">Datasets</h2>

    <div class="dataset-card">
        <h3 class="dataset-title">Synthetic Data for Non-Rigid 3D Reconstruction using a Moving RGB-D Camera</h3>
        <div class="publication-description">
            <img src="https://eshafeeqe.github.io/homepage/shafeeq_files/icpr_dataset.png" alt="ICPR dataset thumbnail" style="width: 200px; height: auto; float: left; margin-right: 1rem; margin-bottom: 1rem;">
            A dataset created for our ICPR 2018 publication, now integrated into the <strong>SLAMBench 3.0</strong> benchmark suite for evaluating SLAM algorithms.
            <br><br>
            <strong>[<a href="https://data.csiro.au/collection/csiro:34677v2" target="_blank"><strong>Link to Dataset</strong></a>]</strong>
        </div>
    </div>

    <div class="dataset-card">
        <h3 class="dataset-title">Synthetic Human Model Dataset for Skeleton Driven Non-Rigid Motion Tracking</h3>
        <div class="publication-description">
            <img src="https://eshafeeqe.github.io/homepage/shafeeq_files/dicta_dataset.png" alt="DICTA dataset thumbnail" style="width: 400px; height: auto; float: left; margin-right: 1rem; margin-bottom: 1rem;">
            A comprehensive dataset for evaluating the reconstruction of fast, articulated human motion, created to validate the work in my DICTA 2018 publication.
            <br><br>
            <strong>[<a href="https://data.csiro.au/collections/#collection/CIcsiro:38398v1/DItrue" target="_blank"><strong>Link to Dataset</strong></a>]</strong>
        </div>
    </div>
</div>
{{< /rawhtml >}}