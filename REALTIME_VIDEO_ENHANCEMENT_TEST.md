# Step-by-Step: Testing Raw vs Enhanced YOLO Detection on Sample Videos

> **Purpose:** Feasibility validation for the Enhanced Vision System. The goal of this test is to determine whether classical image enhancement can improve YOLO object detection on degraded video while keeping processing practical for later real-time use.
>
> **Source:** Test guide provided by Claude and shared by the user.

This assumes Windows. (If you're on Mac, notes are included where commands differ.)
Every step tells you which application to open and what to type/click.

---

## STEP 1 — Install Python

**App: your web browser**

1. Go to https://www.python.org/downloads/
2. Download Python 3.11 (avoid 3.12/3.13 for now — some AI libraries lag behind).
3. Run the installer.
   - On the first install screen, check **"Add Python to PATH"** before clicking Install.
4. Verify:

**App: Command Prompt**
```text
python --version
```

You should see something like `Python 3.11.x`.

---

## STEP 2 — Install VS Code

**App: your web browser**

1. Go to https://code.visualstudio.com/
2. Download and install using the default options.
3. Open VS Code.
4. Install the Microsoft **Python** extension.
5. Install **Jupyter** if desired for viewing images inline.

---

## STEP 3 — Create the Project Folder

Create a workspace such as:

```text
obstacle_project/
├── data/
│   └── raw_videos/
├── results/
```

Open `obstacle_project` in VS Code.

---

## STEP 4 — Add Sample Videos

Put 1–2 short `.mp4` videos (10–30 seconds) into `data/raw_videos/`.

For the first test:

```text
data/raw_videos/normal.mp4
data/raw_videos/lowlight.mp4
```

A normal-lighting clip and a low-light clip are sufficient for the first feasibility test. A person should be visible because YOLO's default model recognizes people.

---

## STEP 5 — Open a VS Code Terminal

In VS Code:

**Terminal → New Terminal**

Use this terminal for the following commands.

---

## STEP 6 — Create a Virtual Environment

```text
python -m venv venv
venv\Scripts\activate
```

On Mac/Linux:

```text
source venv/bin/activate
```

The terminal should show `(venv)`.

---

## STEP 7 — Install Required Libraries

```text
pip install ultralytics opencv-python numpy pandas matplotlib
```

This installs YOLO/Ultralytics, OpenCV, NumPy, Pandas and Matplotlib.

---

## STEP 8 — Create the Baseline Script

Create `run_raw.py` in the project root:

```python
from ultralytics import YOLO
import cv2
import pandas as pd
import time
import os

VIDEO_PATH = "data/raw_videos/lowlight.mp4"
OUTPUT_CSV = "results/raw_lowlight.csv"

model = YOLO("yolov8n.pt")
cap = cv2.VideoCapture(VIDEO_PATH)
rows = []
frame_num = 0

os.makedirs("results", exist_ok=True)

while True:
    ok, frame = cap.read()
    if not ok:
        break
    frame_num += 1

    start = time.time()
    results = model(frame, verbose=False)
    elapsed = time.time() - start

    for box in results[0].boxes:
        cls_id = int(box.cls[0])
        cls_name = model.names[cls_id]
        conf = float(box.conf[0])
        rows.append({
            "frame": frame_num,
            "class": cls_name,
            "confidence": round(conf, 3),
            "fps": round(1 / elapsed, 1) if elapsed > 0 else None
        })

    if frame_num % 30 == 0:
        annotated = results[0].plot()
        cv2.imwrite(f"results/raw_frame_{frame_num}.jpg", annotated)

cap.release()
df = pd.DataFrame(rows)
df.to_csv(OUTPUT_CSV, index=False)
print(f"Done. {len(df)} detections logged to {OUTPUT_CSV}")
```

Run:

```text
python run_raw.py
```

Check that `results/raw_lowlight.csv` and annotated frame images are created.

An empty CSV is still useful information: it may indicate that the degraded footage is too difficult for the detector.

---

## STEP 9 — Create the Enhancement Module

Create `enhance.py`:

```python
import cv2
import numpy as np

def enhance_clahe(frame):
    lab = cv2.cvtColor(frame, cv2.COLOR_BGR2LAB)
    l, a, b = cv2.split(lab)
    clahe = cv2.createCLAHE(clipLimit=2.0, tileGridSize=(8, 8))
    l2 = clahe.apply(l)
    return cv2.cvtColor(cv2.merge((l2, a, b)), cv2.COLOR_LAB2BGR)

def gamma_correct(frame, gamma=1.5):
    inv = 1.0 / gamma
    table = np.array([((i / 255.0) ** inv) * 255 for i in range(256)]).astype("uint8")
    return cv2.LUT(frame, table)

def denoise(frame):
    return cv2.bilateralFilter(frame, 9, 75, 75)

def enhance_pipeline(frame):
    out = gamma_correct(frame, gamma=1.5)
    out = enhance_clahe(out)
    out = denoise(out)
    return out
```

---

## STEP 10 — Create the Enhanced Script

Create `run_enhanced.py`:

```python
from ultralytics import YOLO
from enhance import enhance_pipeline
import cv2
import pandas as pd
import time
import os

VIDEO_PATH = "data/raw_videos/lowlight.mp4"
OUTPUT_CSV = "results/enhanced_lowlight.csv"

model = YOLO("yolov8n.pt")
cap = cv2.VideoCapture(VIDEO_PATH)
rows = []
frame_num = 0

os.makedirs("results", exist_ok=True)

while True:
    ok, frame = cap.read()
    if not ok:
        break
    frame_num += 1

    enhanced = enhance_pipeline(frame)

    start = time.time()
    results = model(enhanced, verbose=False)
    elapsed = time.time() - start

    for box in results[0].boxes:
        cls_id = int(box.cls[0])
        cls_name = model.names[cls_id]
        conf = float(box.conf[0])
        rows.append({
            "frame": frame_num,
            "class": cls_name,
            "confidence": round(conf, 3),
            "fps": round(1 / elapsed, 1) if elapsed > 0 else None
        })

    if frame_num % 30 == 0:
        annotated = results[0].plot()
        cv2.imwrite(f"results/enhanced_frame_{frame_num}.jpg", annotated)

cap.release()
df = pd.DataFrame(rows)
df.to_csv(OUTPUT_CSV, index=False)
print(f"Done. {len(df)} detections logged to {OUTPUT_CSV}")
```

Run:

```text
python run_enhanced.py
```

Check `results/enhanced_lowlight.csv` and the enhanced annotated frames.

---

## STEP 11 — Compare Raw vs Enhanced

Create `compare.py`:

```python
import pandas as pd
import matplotlib.pyplot as plt

raw = pd.read_csv("results/raw_lowlight.csv")
enhanced = pd.read_csv("results/enhanced_lowlight.csv")

raw_count = len(raw[(raw["class"] == "person") & (raw["confidence"] > 0.5)])
enh_count = len(enhanced[(enhanced["class"] == "person") & (enhanced["confidence"] > 0.5)])

raw_conf = raw[raw["class"] == "person"]["confidence"].mean()
enh_conf = enhanced[enhanced["class"] == "person"]["confidence"].mean()

print(f"Raw video -> person detections: {raw_count}, avg confidence: {raw_conf:.2f}")
print(f"Enhanced video -> person detections: {enh_count}, avg confidence: {enh_conf:.2f}")

fig, ax = plt.subplots(1, 2, figsize=(10, 4))
ax[0].bar(["Raw", "Enhanced"], [raw_count, enh_count])
ax[0].set_title("Person detections (conf > 0.5)")
ax[1].bar(["Raw", "Enhanced"], [raw_conf, enh_conf])
ax[1].set_title("Average confidence")
plt.tight_layout()
plt.savefig("results/comparison_chart.png")
plt.show()
```

Run:

```text
python compare.py
```

The comparison is intended to provide evidence about whether enhancement improves detections/confidence on degraded footage.

---

## STEP 12 — Repeat for Other Conditions

Repeat the test for additional clips such as:

- normal lighting
- low light
- fog
- dust
- different object distances

Change `VIDEO_PATH` and `OUTPUT_CSV` for each condition so previous results are not overwritten.

After 4–5 conditions, build a master comparison table from the CSV results.

---

## Feasibility Criteria

The initial test is intended to answer three questions:

1. **Visual feasibility:** Does the enhancement visibly improve degraded frames?
2. **Detection feasibility:** Does enhancement improve useful detection metrics such as detection count and confidence?
3. **Performance feasibility:** Is the processing speed practical enough to justify a later live-camera implementation?

This test does **not** by itself prove real-time performance. The final stage should use a live camera feed and measure end-to-end FPS, latency, and stability.

---

## Troubleshooting

- `python` not recognized → Python was not added to PATH; reinstall and enable the PATH option.
- `pip install` fails → check internet access and package installation errors.
- No detections → confirm the video opens and contains a recognizable object/person; check `cap.isOpened()`.
- Video window closes instantly → expected for these scripts because they process and save results rather than display a live preview.

---

## Transition to Live Testing

Once the sample-video pipeline works, the same processing concept can be adapted to a live camera by replacing the file input with a camera input such as:

```python
cv2.VideoCapture(0)
```

The live test must then measure actual end-to-end performance rather than relying only on inference FPS.
