# MTN Experiment Documentation

## Overview
This project focuses on 3D content generation using diffusion models. The primary goal is to generate 3D objects from text prompts while analyzing common failure modes, the impact of hyperparameters on generation quality and training stability, and the efficiency of the model in terms of memory usage and computation.

## Project Structure
- **docs/xuqiu.md**: Contains a detailed summary and step-by-step guide for the experiment.
- **experiment-record.md**: Outlines the experimental plan and records progress.
- **README.md**: Provides documentation for the project, including setup instructions and usage guidelines.
- **requirements.txt**: Lists the dependencies required for the project.
- **scripts/run_experiment.sh**: A shell script to automate the execution of the experiment.
- **.gitignore**: Specifies files and directories to be ignored by Git.

## Setup Instructions
1. **Clone the Repository**: 
   ```bash
   git clone https://github.com/Texaser/MTN.git
   cd MTN
   ```

2. **Install Dependencies**: 
   Ensure you have Python and pip installed, then run:
   ```bash
   pip install -r requirements.txt
   ```

3. **Install GPU Monitoring Tool**: 
   Install `gpustat` to monitor GPU memory usage:
   ```bash
   pip install gpustat
   ```

4. **Check PyTorch Compatibility**: 
   Verify that your local Python and CUDA versions are compatible with the required PyTorch version.

## Usage Guidelines
- Execute the experiment using the provided shell script:
  ```bash
  bash scripts/run_experiment.sh
  ```

- Follow the instructions in `docs/xuqiu.md` for detailed steps on conducting the experiment, including environment setup, core tasks, and data organization.

## Additional Resources
- [MTN GitHub Repository](https://github.com/Texaser/MTN)
- [PyTorch Previous Versions](https://pytorch.org/get-started/previous-versions)
- [Experiment Video Demonstration](https://www.youtube.com/watch?v=LH6-wKg30FQ)

## Troubleshooting
If you encounter issues during the experiment, refer to the troubleshooting section in `docs/xuqiu.md` for guidance on common problems and their solutions.