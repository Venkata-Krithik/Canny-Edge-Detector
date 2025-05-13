# Canny-Edge-Detector
# 🖼️ Parallelized Canny Edge Detection in SystemC

This project implements a fully pipelined, multithreaded **Canny Edge Detector** using **SystemC**, mimicking real hardware behavior with timing accuracy and modular concurrency. It processes video frames in `.pgm` format and outputs edge-detected images, achieving significant throughput improvement.

---

## 🔧 Features

- ⚙️ **Hardware-Accurate Simulation**: Models real-time edge detection as it would behave on digital hardware.
- 🧵 **Thread-Level Parallelism**: Each image processing stage is pipelined and parallelized (Gaussian blur, gradient computation, suppression, hysteresis).
- 📈 **21× Speedup**: Boosted processing from ~0.9 FPS to 19 FPS by slicing blur and gradient computations into multiple SystemC threads.
- 📹 **Multi-frame Video Support**: Configurable to process multiple frames from a `.pgm` video sequence.

---


Architecture Overview
The processing pipeline consists of the following SystemC modules:

Stimulus: Reads .pgm frames and sends them through FIFO to the pipeline.

Platform: Contains the full pipeline wrapped as a single module:

Gaussian_Smooth: 2D Gaussian blur split into BlurX and BlurY with 4-thread slicing.

Derivative_X_Y: Computes X and Y gradients using Sobel filters.

Magnitude_X_Y: Calculates gradient magnitude using square root approximation.

Non_Max_Supp: Applies directional suppression for sharp edge detection.

Apply_Hysteresis: Applies dual-threshold hysteresis to finalize edges.

Monitor: Receives output image, measures timing, and writes it to disk.

▶️ How to Run
1. Prerequisites
Ensure you have:

SystemC 2.3.3+

A C++17 compatible compiler (e.g., g++, clang++)

PGM images named as Engineering001.pgm, ..., Engineering030.pgm in the video/ folder

2. Compile
bash
Copy
Edit
g++ -I$SYSTEMC_HOME/include -L$SYSTEMC_HOME/lib-linux -lsystemc -lm -std=c++17 -o canny main.cpp
Replace $SYSTEMC_HOME with your actual SystemC path.

3. Run
bash
Copy
Edit
./canny
Output images will be saved as Engineering001_edges.pgm, Engineering002_edges.pgm, etc.

🧠 Core Logic
Gaussian Blur (2D)
Implemented as two passes: BlurX and BlurY

Each split into 4 threads for parallel convolution

Uses 1D kernel generated at runtime with tunable SIGMA

Gradient & Magnitude
Uses integer-based Sobel derivative logic for X and Y

Computes magnitude using sqrt(dx² + dy²) (fixed point approximation)

Non-Maximum Suppression
Checks perpendicular gradient direction to thin edges

Classifies pixels as NOEDGE, POSSIBLE_EDGE

Hysteresis Thresholding
Applies dual thresholding using TLOW and THIGH

Traces connected edge chains to preserve valid features
