https://huggingface.co/datasets/Koolenbrander/assignment-2

---
task_categories:
- video-classification
- object-detection
tags:
- car-parts
- automotive
pretty_name: RAV4 Exterior Parts Video Index
size_categories:
- n < 1K
---

# Car Parts Video Detection Index

This dataset contains a temporal index of car parts detected in https://www.youtube.com/watch?v=YcvECxtXoxQ
Created for Assignment 2 - CS-UY 6613

### Dataset Schema
The Parquet file follows this schema:
| Column | Description |
| --- | --- |
| `video_id` | Identifier for the source video. |
| `timestamp` | Time where the part was detected. |
| `class_label` | The name of the car part. |
| `bounding_box` | Coordinates in `[x_min, y_min, x_max, y_max]` format. |
| `confidence_score` | The model's confidence in the detection (0.0 - 1.0). |

### Model Information
- **Base Model:** YOLOv8n
- **Fine-tuning:** Trained on the Ultralytics Car Parts dataset (5 epochs).