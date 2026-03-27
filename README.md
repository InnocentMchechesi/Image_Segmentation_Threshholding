# 🖼️ Image Segmentation via Thresholding

A hands-on exploration of **8 classical image segmentation techniques** using OpenCV and scikit-image. Each method produces a binary mask and draws bounding boxes around detected objects — making it easy to compare how different threshold strategies perform on the same image.

---

## 📁 Files

| File | Description |
|------|-------------|
| `segment.py` | Self-contained Python script — run once, outputs a full comparison figure |
| `image_segmentation_thresholding.ipynb` | Interactive Jupyter notebook with one cell per method, inline plots, and tunable parameters |

---

## 🔬 Methods Covered

| # | Method | Type | Best For |
|---|--------|------|----------|
| 1 | **Global (fixed t=127)** | Global | Baseline / controlled environments |
| 2 | **Otsu's Method** | Global | Bimodal histograms (clear fg/bg split) |
| 3 | **Triangle** | Global | Unimodal histograms / sparse objects |
| 4 | **Li (Cross-Entropy)** | Global | Natural photographs |
| 5 | **Niblack** | Local | Uneven illumination |
| 6 | **Sauvola** | Local | Lighting gradients and shadows |
| 7 | **HSV Color Segmentation** | Color | Known-color backgrounds |
| 8 | **Canny Edge → Fill** | Edge-based | Strong edges, low intensity contrast |

Every method applies **morphological cleanup** (close + open) before contour detection to reduce noise and improve bounding box quality.

---

## 🚀 Getting Started

### 1. Clone the repo

```bash
git clone https://github.com/your-username/your-repo-name.git
cd your-repo-name
```

### 2. Install dependencies

```bash
pip install opencv-python-headless scikit-image matplotlib numpy
```

### 3a. Run the script

```bash
python segment.py
```

Outputs `segmentation_results.png` — a full grid comparing all 8 methods.

### 3b. Run the notebook

```bash
jupyter notebook image_segmentation_thresholding.ipynb
```

Each method has its own cell with explanations, tunable parameters, and inline visualisations.

---

## ⚙️ Configuration

At the top of both files, two variables control behaviour:

```python
IMG_PATH = "my_img.jpeg"   # path to your input image
MIN_AREA = 3000            # minimum contour area in px² to count as an object
```

In the notebook, each method also exposes its own tunable parameters directly in the cell — for example:

```python
# Niblack / Sauvola
WINDOW = 51   # local window size — try 25, 51, 101
K      = 0.2  # sensitivity — try 0.1 to 0.5

# HSV Color Segmentation
blue_lo = np.array([ 90,  80,  80])  # HSV lower bound
blue_hi = np.array([135, 255, 255])  # HSV upper bound

# Canny Edge
low_t  = 30    # lower hysteresis threshold
high_t = 100   # upper hysteresis threshold
```

---

## 🧠 Method Notes

**Global & Otsu** work by finding a single intensity value that splits the whole image into foreground and background. Otsu automates this by maximising inter-class variance — a good first choice when the histogram is bimodal.

**Triangle & Li** are also single-value methods but use different criteria. Triangle draws a geometric line from the histogram peak to its tail; Li minimises cross-entropy. Both tend to outperform Otsu on natural images with more complex histograms.

**Niblack & Sauvola** compute a *local* threshold for every pixel based on the mean and standard deviation within a sliding window. This makes them robust to shadows and uneven lighting that would fool any global method. Sauvola adds a normalisation term that generally makes it more stable than Niblack.

**HSV Color Segmentation** bypasses intensity entirely, working directly in hue-saturation-value space. When the background has a uniform, known colour (as in a flat-lay studio photo), this approach is often the most accurate.

**Canny → Fill** takes an edge-detection approach: detect strong edges, dilate them to close gaps, then flood-fill enclosed regions. Useful when objects share a similar intensity with the background but have clear boundaries.

---

## 📦 Dependencies

- [OpenCV](https://opencv.org/) (`opencv-python-headless`)
- [scikit-image](https://scikit-image.org/)
- [NumPy](https://numpy.org/)
- [Matplotlib](https://matplotlib.org/)

Python 3.8+ recommended.

---

## 📄 License

MIT
