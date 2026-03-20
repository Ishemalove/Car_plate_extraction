# Real-time Car Number Plate Extraction

A real-time computer vision pipeline for detecting, aligning, and extracting text from car license plates using OpenCV and Tesseract OCR.

---

## Features

- **Real-time Detection**: Uses edge detection and contour analysis to locate plates.
- **Perspective Alignment**: Automatically deskews plates into a rectangular format for better OCR accuracy.
- **Robust OCR**: Uses Tesseract with custom configurations and Otsu thresholding.
- **Temporal Validation**: Implements majority voting across multiple frames to reduce noise and flickering.
- **Auto-Logging**: Validated plate numbers are saved to a CSV file with timestamps.

---

## Pipeline Overview

### 1. Detection Stage (`src/detect.py`)
Identifies potential plate regions in the video stream.
- Converts frame to grayscale and applies Gaussian blur.
- Uses Canny Edge Detection to find contours.
- Filters candidates based on Area (>600 units) and Aspect Ratio (2.0 to 8.0).

### 2. Alignment & OCR (`src/ocr.py`)
Standardizes the detected plate and reads the text.
- Applies a Perspective Transform to warp the plate into a fixed 450x140 rectangle.
- Preprocesses the warped image with Grayscale and Otsu's Thresholding.
- Extracts text using Pytesseract (configured for Page Segmentation Mode 8 - single word).

### 3. Validation & Temporal Logic (`src/temporal.py`)
Ensures high confidence before logging.
- **Regex Validation**: Only matches plates following the `[A-Z]{3}[0-9]{3}[A-Z]` pattern.
- **Majority Voting**: Maintains a buffer of the last 5 valid results and picks the most frequent.
- **Cooldown Logic**: Prevents duplicate logging of the same plate within a 10-second window.

---

## Installation

1. **Install Tesseract OCR Engine**:
   - **Linux**: `sudo apt install tesseract-ocr`
   - **macOS**: `brew install tesseract`
   - **Windows**: Download the installer from the Tesseract GitHub repository.

2. **Clone and Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

---

## Usage

Run the temporal validation script for the full pipeline:
```bash
python src/temporal.py
```

Other utility scripts:
- `python src/detect.py`: Test only the plate detection stage.
- `python src/ocr.py`: Test detection + OCR alignment.
- `python src/validate.py`: Test detection + OCR + Regex validation.

---

## Project Structure

- `src/`: Core Python source files.
- `data/plates/`: Location of the generated `log.csv`.
- `data/screenshots/`: Storage for project result captures.
- `requirements.txt`: Python dependencies.
