# Canny-Edge-Detector
Overview
This repository contains a SystemC implementation of the Canny Edge Detector, adapted from the original C++ source code. The SystemC version has been enhanced with several optimizations to achieve real-time performance on hardware platforms like the Raspberry Pi 5. Key improvements include pipelining, parallelization, and precision adjustments.


Features:
Pipelined Process
The various stages of the Canny Edge Detection algorithm (e.g., gradient computation, non-maximum suppression, double thresholding) are implemented in a pipelined fashion to optimize throughput.

Parallelized Functions:
Independent components of the algorithm are parallelized, leveraging the concurrency features of SystemC.

Real-Time Delay Estimation:
To model real-world performance, the C++ version of the code was cross-compiled and executed on a Raspberry Pi 5. Measured delays for key functions were integrated into the SystemC model for accurate timing behavior.

Improved Efficiency with Integer Arithmetic:
Some floating-point operations were replaced with integer arithmetic to improve efficiency while preserving the accuracy of the application. Only critical floating-point operations were retained to ensure the application remains robust.
