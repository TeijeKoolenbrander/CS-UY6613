Deliverables:
Video 1
https://youtu.be/Sb-pa6iv1YI
Video 2
https://youtu.be/ZV62udKJGeE
Hugging Face: 
https://huggingface.co/datasets/Koolenbrander/assignment-3

1. Dataset Choice & Detector Configuration
    Dataset: The detector was trained using a custom drone dataset from roboflow, consisting of high-angle and low-angle aerial imagery of various drone models.
    Model: YOLOv8 Nano. Chosen to balance high-speed inference  with a small memory footprint suitable.Configuration: Image size 640x640 pixels.
    Confidence Threshold: 0.5 (to minimize false positives).
    Training: Fine-tuned for 25+ epochs until the mAP stabilized.

2. Kalman Filter State Design & Noise Parameters
    To ensure smooth tracking, a Linear Kalman Filter was implemented using filterpy. 
    The filter tracks drone's position and velocity in a 2D coordinate system:
    x = [u,v,u*,v*]^T where u,v are pixel coordinates of bounding box center and u*, v& are the horizontal and vertical velocity.

    Noise Parameters:
    State Transition: A Constant Velocity model assumes the drone moves at a steady pace between frames.
    Measurement Noise: Set to 5. High level of trust in the YOLO’s accuracy but allows slight jitter in the bounding box.
    Process Noise: Set to 0.1. Small value accounts for "wobble" or slight deviations.
    Covariance: Initialized to 1000, allows the filter to go to the first detection almost instantly.

3. Failure Cases & Handling Missed Detections
    The primary challenge in drone tracking is occlusion or motion blur. 
    Handling Missed Detections:
    The system handles misses by separating the Predict and Update steps. In every frame, the Kalman Filter predicts the next position based on the last known velocity. If YOLO finds the drone, the filter updates its internal state with the new data. If YOLO misses the drone, the update step is skipped. The trajectory line continues to move based on the drone's momentum which allows the tracker to successfully bridge gaps of 5–10 frames where the drone could potentially not be visible.
    Known Failure Cases:
    If the drone performs an aggressive 90-degree turn while occluded, the Constant Velocity model will predict it continuing straight, causing a lag in the trajectory until the next detection corrects the state. If an object similar to a drone is detected with high confidence near the drone's path, the Kalman Filter may temporarily "hitch" toward the wrong object creating a false positive.
