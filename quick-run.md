## 📌 Quick Run: YOLOv5 Object Detection Demo

This guide walks you through setting up and running YOLOv5 for object detection using images, videos, and live webcams. It also covers how to analyze detection metrics and send results to an external system.

### 📍 1. Installation

1. Clone the YOLOv5 repository (your forked version):

    git clone https://github.com/YOUR_GITHUB_USERNAME/yolov5.git
    cd yolov5

    (Replace YOUR_GITHUB_USERNAME with your actual GitHub username.)

2. Install dependencies:

    ```bash
    pip install -r requirements.txt
    ```

3. Download a pretrained YOLOv5 model:

    ```bash
    wget https://github.com/ultralytics/yolov5/releases/download/v6.0/yolov5s.pt
    ```

    (You can replace yolov5s.pt with yolov5m.pt, yolov5l.pt, or yolov5x.pt for different model sizes.)

### 📍 2. Run Object Detection

* Test on an Image

    ```
    python detect.py --weights yolov5s.pt --img 640 --conf 0.5 --source image.jpg
    ```

    (Make sure image.jpg is inside the yolov5 folder or provide the full path.)

* Test on a Video

    ```
    python detect.py --weights yolov5s.pt --img 640 --conf 0.5 --source video.mp4
    ```

    (Replace video.mp4 with your actual video file.)

* Test on a Webcam

    ```bash
    python detect.py --weights yolov5s.pt --img 640 --conf 0.5 --source 0
    ```

    (Use source 1 if you have multiple webcams.)

### 📍 3. Get Performance Metrics

* Measure Inference Speed

    ```bash
    python detect.py --weights yolov5s.pt --img 640 --conf 0.5 --source image.jpg --profile
    ```

    This will output detection time per frame, FPS (frames per second), and model latency.

* Save Detection Results

    To save detections in text format:

    ```bash
    python detect.py --weights yolov5s.pt --img 640 --conf 0.5 --source image.jpg --save-txt
    ```

    This creates a `.txt` file in `runs/detect/exp/labels/` containing detected object classes and bounding box coordinates.

### 📍 4. Sending Detection Data to an External System

* Option 1: Save Results to a JSON File

    Modify `detect.py` to output detections as JSON:

    ```python
    import json

    def save_results_as_json(results, output_file="detections.json"):
        with open(output_file, "w") as f:
            json.dump(results, f, indent=4)
    ```

    Call this function inside `detect.py` after object detection.

* Option 2: Send Data to a Web API

    If you want to send detection results to a server, modify `detect.py` to include:

    ```python
    import requests

    def send_results_to_api(results, api_url="http://example.com/api"):
        response = requests.post(api_url, json=results)
        print(f"Response: {response.status_code}, {response.text}")
    ```

    Replace `"http://example.com/api"` with your actual API endpoint.

### 📍 5. Troubleshooting

* 🔴 FileNotFoundError: [image.jpg] does not exist

    ➡️ Check if the image is in the correct directory. Use:

    `ls -l image.jpg`

    ➡️ If not found, move it inside yolov5/:

    `mv /path/to/image.jpg yolov5/`

* 🔴 Torch not found?

    ➡️ Try installing PyTorch:

    `pip install torch torchvision torchaudio`

* 🔴 YOLO runs too slow?
    
    ➡️ Run it on a GPU with CUDA (if available):
    
    `python detect.py --weights yolov5s.pt --img 640 --conf 0.5 --source image.jpg --device 0`