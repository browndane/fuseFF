# Multi-Modal Biometric Recognition Framework

A comprehensive C++ framework for multi-modal biometric feature extraction, enhancement, fusion, and classification. Originally developed for research involving face, fingerprint, iris, and palm recognition, this pipeline supports multiple matching algorithms including Eigenfaces, Fisherfaces, Local Binary Patterns Histograms (LBPH), Support Vector Machines (SVM), and Logistic Regression (LR).

## Features

* **Image Enhancement Pipeline:** Includes Histogram Equalization, Laplacian of Gaussian (LoG), Gaussian Blurring, CLAHE, and Non-Local Means Denoising.
* **Feature Extraction:** Implements standard and variant Local Binary Patterns (ELBP, VARLBP, OLBP), PCA dimension reduction, and Gabor filtering.
* **Modality Fusion:** Supports both horizontal concatenation and weighted score-level/feature-level fusion across multiple biometric modalities.
* **Classification Matching:** Leverages OpenCV's `FaceRecognizer` framework (Eigen, Fisher, LBPH) and interfaces with `libsvm` for SVM and Logistic Regression training/prediction.
* **Automated Evaluation:** Calculates comprehensive biometrics metrics including Detection and Identification Rate (DIR), False Positive Identification Rate (FPIR), False Rejection Rate (FRR), and Miss Rate (MR) across iterative thresholds.

## Prerequisites

* **C++ Compiler:** GCC or Clang (C++11 support recommended).
* **OpenCV (2.4.x):** Requires `core`, `contrib`, `imgproc`, and `highgui` modules. *(Note: The `contrib` module was moved to `opencv_contrib` in OpenCV 3+, so OpenCV 2.x is strictly required for `FaceRecognizer` out-of-the-box).*
* **LibSVM:** The executables `./train` and `./predict` (or `./svm-train` and `./svm-predict` renamed) must be accessible in the execution directory for SVM/LR matching.
* **CMake:** For building the project (minimum version 3.10).

## Building the Project

```bash
git clone https://github.com/[Your-Username]/[Repository-Name].git
cd [Repository-Name]
mkdir build && cd build
cmake ..
make

```

*Ensure your `../cmakeStyle/lbp.hpp` custom headers and LibSVM binaries are properly configured in your `CMakeLists.txt` or system paths.*

## Dataset Format

The application expects input datasets defined via CSV files. The CSV format should be separated by semicolons (`;`) containing the absolute/relative image path and the integer class label.

**Example `dataset.csv`:**

```text
/path/to/dataset/subject1_img1.jpg;0
/path/to/dataset/subject1_img2.jpg;0
/path/to/dataset/subject2_img1.jpg;1

```

## Usage

The executable requires exactly 12 command-line arguments to dictate the execution pipeline.

```bash
./biometric_fusion <csv1> <csv2> <samples> <model_name> <matcher> <enhance> <reduction> <variance> <fusion> <roi_size> <classes> <threshold>

```

### Arguments

| Argument | Type | Description |
| --- | --- | --- |
| `csv1` | string | Path to the primary modality dataset CSV. |
| `csv2` | string | Path to the secondary modality dataset CSV (use the same path as `csv1` if no fusion is required). |
| `samples` | int | Number of training samples to allocate per person/class. |
| `model_name` | string | Output base name for the generated model and results text file. |
| `matcher` | string | Matching algorithm: `E` (Eigen), `F` (Fisher), `L` (LBPH), `SVM`, or `LR`. |
| `enhance` | int | Enhancement type:<br>

<br>`1`: Normalization<br>

<br>`2`: EqualizeHist<br>

<br>`3`: Hist + LoG<br>

<br>`4`: LoG only<br>

<br>`5-7`: Advanced LBP combinations |
| `reduction` | string | Dimensionality reduction type: `None`, `PCAa`, `PCAb`, or `Gabor`. |
| `variance` | float | Retained variance percentage for PCA or parameters for Gabor filters. |
| `fusion` | int | Fusion strategy: `1` (Weighted Average) or `2` (Horizontal Concatenation). |
| `roi_size` | int | Target Region of Interest (ROI) size (e.g., `30` for 30x30 pixels). Images are optimally resized. |
| `classes` | int | Total number of distinct classes. Use `-1` for closed-set identification. |
| `threshold` | float | Starting threshold for the matching algorithm's metric iteration. |

### Example Execution

Run an SVM fusion task on face and fingerprint datasets, retaining 95% variance via PCA, using 5 training samples per class for 100 classes:

```bash
./biometric_fusion faces.csv fingers.csv 5 output_model SVM 1 PCAa 95 2 100 100 0.0

```

## Output

The program automatically splits the CSV into training and testing subsets based on the `<samples>` parameter. Outputs include:

* Trained model files (`.model` for LibSVM or internal OpenCV models).
* A `.txt` output file corresponding to the `<model_name>` containing raw prediction data.
* Standard output (console) logs detailing continuous evaluation metrics (MR, FPIR, DIR) over calculated thresholds.

## Citation

If you utilize this framework in your academic work, please cite the author:

```bibtex
@misc{brown2017multimodal,
  author = {Brown, Dane},
  title = {Multi-Modal Biometric Recognition Framework},
  year = {2017},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/[Your-Username]/[Repository-Name]}}
}

```

## License

Copyright (c) 2017 Dane Brown. All rights reserved.
No warranty, explicit or implicit, provided.

---

*For issues, bug reports, or contributions, please open an issue or submit a pull request.*
