# Pokemon_pregrading_ai

Demo and methodology for an AI system to detect Pokémon card defects and predict PSA-style grades. Includes multi-angle image handling and annotated visual outputs. Dataset and proprietary images are private.

AI Pokémon Card Pre-Grading Tool (Demo & Methodology)

Author: Ajay Ahluwalia
Status: In Progress
Note: All code and methodology are the intellectual property of Ajay Ahluwalia. Datasets, PSA slab images, and full AI models are not included. Demo uses synthetic or placeholder images for demonstration purposes only.

Project Overview

This project is developing an AI-powered Pokémon card pre-grading system that provides collectors and enthusiasts with:

Automated defect detection (scratches, whitening, print lines, ink dots, dents)

PSA-style pre-grade estimation (1–10 scale)

Annotated images highlighting defects with confidence scores

Support for multi-angle images for more accurate analysis

The goal is to help collectors decide whether to submit their cards for professional grading, reducing uncertainty and potential costs.

Problem Statement
Many collectors have the issue wether or not to submit to psa for grading since it costs money and if the grade isn't a 10 you will lose money. Currently, there are no AI tools that reliably detect fine defects, calculate a PSA-style grade, or provide explainable visual feedback.

This project aims to fill that gap by providing a pre-grading AI system that is explainable, practical, and continuously improving.

Objectives-

Detect card defects accurately with high confidence, Aggregate multiple images per card for better detection, Provide explainable outputs with annotated images
Generate PSA-style grades based on defects, centering, and surface metrics, Collect user-submitted PSA grades for validation and continuous improvement, Provide a demo that can showcase functionality without exposing proprietary data

Solution Overview

The AI system workflow:

1. Data Collection
   Public images of raw pokemone cards

2. Preprocessing
   Card detection and alignment with OpenCV and Normalization of size, lighting, and orientation

3. Defect Detection
   YOLOv8 model for detecting scratches, whitening, print lines, ink dots, and dents
   Outputs bounding boxes with defect labels and confidence scores

4. Multi-Image Aggregation
   Supports multiple front/back images per card
   Aggregates defects using maximum confidence per type

5. Centering & Surface Analysis
   Measures card centering (left/right, top/bottom)
   Optional CNN for subtle surface wear

6. PSA-style Grade Aggregation
   Combines defect detection, centering, and surface analysis
   Outputs predicted grade (1–10) and annotated images

7. Validation Loop
   Users may submit actual PSA grades
   Enables continuous improvement and comparison of predictions vs real grades

Patch-Based Detection

To improve detection of small or subtle defects such as scratches, ink dots, whitening, or fine print lines, the system uses a patch-based approach:

Splitting images into patches and each card image is divided into smaller tiles (4×4 or 8×8 sections).

Each patch is processed individually by the AI model.

Advantages:
Higher defect resolution: small defects are easier to detect on zoomed-in patches.
Better model focus: reduces false negatives by simplifying the input per patch.
Memory efficiency: smaller patches reduce GPU memory usage during inference.

Challenges:
Context loss: defects spanning multiple patches may need careful aggregation.
More computation: processing multiple patches takes longer than one full image.
Aggregation required: predictions from each patch must be merged to reconstruct the full card annotation.

Integration with multi-angle images:
Each patch from every uploaded angle is processed, then aggregated across angles to maximize detection accuracy.
Slight overlap between patches can improve detection at boundaries.

Technical Stack

## Technical Stack

| Component                  | Technology                        | Purpose                                             |
| -------------------------- | --------------------------------- | --------------------------------------------------- |
| Programming Language       | Python 3.10+                      | Core development                                    |
| Object Detection           | YOLOv8 (PyTorch)                  | Detect defects on card images                       |
| Computer Vision            | OpenCV, PIL                       | Preprocessing, alignment, annotation                |
| Dataset Labeling           | Roboflow                          | Annotate defects and export YOLO-formatted datasets |
| Rule-based PSA Aggregation | Python                            | Calculate PSA-style grades                          |
| Database                   | PostgreSQL                        | Store user-submitted grades                         |
| Validation                 | PSA slabs + synthetic/demo images | Compare predictions to real data                    |
| Mobile Development         | Android / iOS                     | Allows users to access pre-grading AI on mobile     |
