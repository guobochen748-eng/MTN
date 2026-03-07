# Experimental Plan and Progress Record

## Experiment Overview
This experiment focuses on 3D content generation using diffusion models, specifically aiming to generate 3D objects from text prompts. The core objectives include understanding the generation process, identifying common failure modes, and analyzing the impact of hyperparameters on the results.

## Key Information
- **Core Code Repository**: [MTN GitHub Repository](https://github.com/Texaser/MTN)
- **Submission Deadline**: March 15, 23:59
- **Submission Platform**: UM Moodle
- **Required Tools**: 
  - `gpustat` for GPU memory monitoring
  - PyTorch and other dependencies as listed in `requirements.txt`

## Experimental Steps

### Step 1: Environment Setup
- Confirm compatibility of Python, CUDA, and PyTorch versions.
- Clone the MTN repository and install dependencies.
- Install `gpustat` for monitoring GPU usage.
- Adjust resolution settings if GPU memory is limited.

### Step 2: Core Experimental Tasks
1. **Task 1**: Generate 3D video from text prompt.
   - Record the command used and the time taken for generation.
   - Save the generated video and log files.
   
2. **Task 2**: Monitor GPU memory usage.
   - Record initial and final GPU memory states.
   - Create a table of memory usage over time during the generation process.
   
3. **Task 3**: Identify and analyze failure cases.
   - Document at least two types of failures observed in the generated 3D objects.
   - Include screenshots and descriptions of the issues.
   
4. **Task 4**: Adjust hyperparameters and observe effects.
   - Modify one hyperparameter and document the changes in results and performance.

### Step 3: Data Organization
- Compile all commands executed, GPU memory usage data, visual materials (screenshots), and analyses of failures and hyperparameter adjustments.

### Step 4: Report Writing
- Prepare a LaTeX report following the ACM format, including:
  - Introduction and objectives
  - Detailed descriptions of tasks and observations
  - Analysis of failures and proposed improvements
  - Reflection on hyperparameter sensitivity

### Step 5: Submission and Backup
- Ensure the report is submitted by the deadline.
- Backup all relevant files, including code, generated videos, and LaTeX project files.

### Step 6: Troubleshooting
- Address any issues encountered during the experiment, such as memory errors or code failures, by following the troubleshooting guidelines provided in `xuqiu.md`.

## Progress Record
- **[Date]**: Initial environment setup completed.
- **[Date]**: Task 1 executed; 3D video generated successfully.
- **[Date]**: GPU memory monitoring conducted; data recorded.
- **[Date]**: Failure cases identified and documented.
- **[Date]**: Hyperparameter adjustments made; results analyzed.
- **[Date]**: LaTeX report drafted and under review.
- **[Date]**: Final submission completed. Backup performed. 

This document will be updated regularly to reflect ongoing progress and findings throughout the experiment.