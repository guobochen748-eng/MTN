# CISC7202 Experiment Record

## Experimental Plan

### Experiment Overview
This experiment focuses on **3D content generation using diffusion models**. The primary goal is to generate 3D objects from text prompts while analyzing common failure modes, the impact of hyperparameters on generation quality and training stability, and the efficiency of the model in terms of memory usage and computation.

### Key Information
- **Core Code Repository**: [MTN GitHub Repository](https://github.com/Texaser/MTN)
- **Reference Materials**: 
  - [Experiment Video Demonstration](https://www.youtube.com/watch?v=LH6-wKg30FQ)
  - [PyTorch Version Compatibility](https://pytorch.org/get-started/previous-versions)
- **Environment Tools**: Install `gpustat` for monitoring GPU memory usage.
- **Important Notes**: 
  - Do not delete or rename checkpoint files after training.
  - Backup checkpoint files before retraining to avoid overwriting.

## Step-by-Step Progress

### Step 1: Environment Setup
- Confirm PyTorch compatibility with local Python and CUDA versions.
- Clone the MTN repository:
  ```bash
  git clone https://github.com/Texaser/MTN.git
  ```
- Install required dependencies:
  ```bash
  pip install -r requirements.txt
  ```
- Install GPU memory monitoring tool:
  ```bash
  pip install gpustat
  ```

### Step 2: Core Experiment Tasks
#### Task 1: Generate 3D Video from Text Prompt
- Selected prompt: `a tiger dressed as a doctor`
- Execute the core command for 3D generation and record the command used.

#### Task 2: Monitor GPU Memory Usage
- Record initial GPU memory state using `gpustat`.
- Monitor memory usage at regular intervals during the generation process.

#### Task 3: Identify and Analyze Failure Cases
- Review generated 3D video for at least two types of failures (e.g., geometric distortion, missing parts).
- Document findings with screenshots.

#### Task 4: Hyperparameter Adjustment
- Modify one hyperparameter (e.g., learning rate) and observe the effects on generation quality and training stability.

### Step 3: Data Organization
- Compile command records, GPU memory usage data, visual materials, and analysis of failure cases.
- Prepare for LaTeX report writing.

### Step 4: LaTeX Report Writing
- Follow the ACM format for the report, ensuring all required sections are included.

### Step 5: Submission and Backup
- Submit the report by the deadline of March 15, 23:59.
- Backup all relevant files, including code, generated videos, and LaTeX project files.

### Step 6: Troubleshooting
- Address any issues encountered during the experiment, such as memory errors or code failures, by following the outlined troubleshooting steps.

## Progress Tracking
- Document progress and findings in this file as the experiment proceeds, ensuring all steps are recorded for future reference.