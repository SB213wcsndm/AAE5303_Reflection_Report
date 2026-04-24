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

## Section 6: Critical Reflection

The application of artificial intelligence has greatly facilitated my study. Cursor is highly effective in helping me troubleshoot errors and provide guidance for the next step, enabling me to fully understand the causes and outcomes of each operation. It has significantly assisted me in completing all assignments and projects.

On the other hand, I found that Cursor’s AI Agent will modify my files automatically based on its generated outputs, including files in the workspace. For instance, during the training initialization phase of the 3D scene reconstruction project, the Agent arbitrarily altered the required pose .bin file in the workspace. This mismatch between the actual images and the modified pose .bin file prevented the training process from starting successfully. This abnormal issue was only resolved after I deleted and reconstructed the relevant files and datasets.

If I were to redo this project, I would first figure out the exact meaning of each operational step before allowing AI to intervene in specific tasks. Meanwhile, I would isolate workspace files in advance, so that I can collaborate with AI to troubleshoot errors encountered in the project more reliably.

---

## Section 7: Evidence

### 7.1 Comand Snippets

AI-suggested comand and their meaning:

Cursor suggested this to reduced inter-image matching parameters to accelerate runtime performance on the CPU.
```bash
Feature Extraction:
colmap feature_extractor \             --Invoke the feature_extractor command of COLMAP. 
  --database_path database.db \
  --image_path images \
  --ImageReader.single_camera 1 \              --The same camera intrinsic model 
  --SiftExtraction.use_gpu 0 \                 --Disable GPU and use CPU only
  --SiftExtraction.max_image_size 1000 \       --Limit the long side of images to a maximum of 1000 pixels
  --SiftExtraction.max_num_features 1024 \     --Retain a maximum of 1024 feature points per image
  --SiftExtraction.num_threads 2               --Only use 2 CPU threads

Feature Matching
colmap sequential_matcher \             --Invoke the sequential matching module of COLMAP，instead of exhaustive_matcher for a rapid result 
  --database_path database.db \
  --SiftMatching.use_gpu 0 \                --Disable GPU and use CPU only
  --SequentialMatching.overlap 15 \         --Up to approximately 15 preceding and subsequent frames
  --SiftMatching.max_num_matches 16384 \    --A maximum of 16384 matching points will be retained
  --SiftMatching.num_threads 2              --Only use 2 CPU threads



```

### 7.2 Prompt Examples

Prompt:  error bash

Result: 
This indicates that the feature matches among the 20 images are too few and too weak. The mapper cannot find a valid initial image pair to start reconstruction, hence it keeps reporting the error: No good initial image pair found.
Brief Causes
1. You are using sequential_matcher with overlap=3, which is still too strict for some UAV continuous image sequences.
2. The 20 images are selected simply by filename order instead of chronological continuity, resulting in insufficient overlap between adjacent frames.
3. The feature configuration is relatively conservative (with max_image_size set to 1000 or 1024). Matching becomes even more difficult in weakly textured scenes.

