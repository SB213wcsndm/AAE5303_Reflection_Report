# AAE5303_Reflection_Report

**Student Name:** ZHANG Yuhao

**Student ID:** 25044023G

**Group Number:** 我們組 (WMZ)

**Date:** April 24, 2026

## Section 1: AI Usage Experience

In the two assignments and the 3D scene reconstruction project, I mainly used Cursor as my development tool.

First, for Assignment 1, I updated the Python environment following Cursor’s suggestions. I previously had Ubuntu 20.0.4 and ROS1 Noetic installed, which came with Python 3.8.10. Therefore, I needed to upgrade Python to version 3.10.14, and I followed the step-by-step guidance provided by Cursor throughout the process.

In Assignment 2, I also relied on Cursor to fix the docker run command, enabling me to launch Xlaunch inside a Docker container and successfully complete the task.

For the final 3D scene reconstruction project, Cursor assisted me in tuning the parameters to best match my device configuration.

---

## Section 2: Understanding AI Limitations

When using Cursor as the development tool to complete the 3D scene reconstruction project, due to the limitations of my device, the AI suggested reducing the image matching parameters to improve the running speed under CPU computation. However, after following this suggestion, the matching results between images became too sparse and weak. Consequently, the subsequent mapper command failed to find a valid initial image pair to start the reconstruction process, making it impossible to proceed with the subsequent reconstruction workflow.

Therefore, it was necessary to adjust the inter-image matching parameters to balance computational efficiency and matching quality — ensuring both acceptable running speed and sufficient feature strength for reliable image matching. By combining feedback from Cursor AI, I increased the overlap parameter and the feature matching quantity parameter, then re-ran the reconstruction pipeline, which successfully resolved the issue.

---

## Section 3: Engineering Validation

During the 3D scene reconstruction project, Cursor assisted me in writing the commands for generating 3D model files. I additionally added a setting to produce intermediate output files every 10 training iterations, allowing me to evaluate the reconstruction quality more intuitively.

The generated intermediate results clearly showed that the model after 10 training iterations suffered from severe feature loss, with obvious missing details in roads, water surfaces, flat ground and other areas. In contrast, feature missing was greatly reduced after 30 iterations.

I then ran the Python script provided in the project demo to obtain the evaluation curve. The results indicated that the feature loss decreased by 36.7% after 30 training rounds, dropping from the initial value of 0.2798 to 0.1772. This verified that the reconstruction performance achieved a substantial improvement in this scenario.

---

## Section 4: Problem-Solving Process

During the 3D scene reconstruction project, I encountered a series of major issues that caused my computer to freeze and crash unexpectedly. The root cause was that my device is equipped with an AMD CPU rather than a CUDA-capable GPU. Directly executing the reconstruction commands led to excessively high CPU usage and eventually a system breakdown.

By referring to the official OpenSplat open-source tutorials, I learned how to build OpenSplat for CPU-only operation. Meanwhile, I consulted Cursor and obtained optimized commands that reduced inter-image matching parameters to accelerate runtime performance on the CPU.

Although this solution allowed me to continue the project, following the suggested parameters resulted in too few and weak feature matches between images. Consequently, the subsequent mapper module could not locate a valid initial image pair to initiate reconstruction, making further training impossible.

I therefore needed to fine-tune the image matching parameters to strike a balance: maintaining acceptable computation speed while preserving sufficient feature robustness for reliable image registration. With guidance from Cursor, I increased the overlap threshold and the number of feature matches, then re-ran the pipeline. This adjustment successfully generated the final 3D scene reconstruction model file.

---

## Section 5: Learning Growth

Before taking this course, I had already learned how to use ROS1 Noetic, but I had no prior experience with Docker or Cursor for development. During Assignment 1, I started using Cursor to help troubleshoot and resolve technical errors. For Assignment 2, I pulled Docker images and configured proper docker run commands with Cursor’s guidance to complete the project tasks.

My most significant progress lies in my in-depth understanding of the entire 3D scene reconstruction workflow. I have gained a clear grasp of how each step connects as a complete pipeline, including building the OpenSplat development environment, importing training datasets, feature extraction, feature matching, pose generation, and 3D scene reconstruction. Furthermore, I am now able to comprehend the specific functions and implications of relevant parameters and commands throughout the process, and adjust them appropriately according to actual project requirements.

---

## Section 6: Learning Growth



---

## Section 7: Evidence

### 7.1 Code Snippets

AI-suggested IMU refinement (Misleading):

Gemini suggested this to "fix" the leftward drift
```bash
Tbc: !!opencv-matrix
   data: [ -0.008726, -0.999924,  0.008727, -0.00674,
            0.008727, -0.008726, -0.999924, -0.02840,
            0.999924,  0.008650,  0.008803, -0.09965,
            0.0,       0.0,       0.0,       1.0]
```
Note: This still induced drift because the rotation matrix was not aligned with the actual drone specification.

### 7.2 Prompt Examples

Prompt: "Create a Google Slides for my presentation of 5-6 minutes regarding the refinements of visual odometry... and point out I used Apple Silicon via UTM rather than Docker."

Result: Helpful for structure, but failed to include the specific technical figures (intrinsic/extrinsic parameters) required for a professional engineering presentation.

### 7.3 Accuracy Evidence

Leaderboard Achievement: Successfully reached a score of 92.6 by scaling ORBextractor.nFeatures to 15,000, validating that a high-density feature approach can compensate for certain inaccuracy in SLAM.
