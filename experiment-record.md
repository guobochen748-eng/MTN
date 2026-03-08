# CISC7202 实验记录（MTN 文本到3D）

## 1. 实验信息
- 课程：CISC7202
- 任务：Experiment Report-3（3D Generation）
- 截止时间：2026-03-15 23:59（UM Moodle）
- 项目：MTN（Multi-Scale Triplane Network）
- 仓库地址：https://github.com/Texaser/MTN
- 参考视频：https://www.youtube.com/watch?v=LH6-wKg30FQ

## 2. 实验目标（依据 `xuqiu.md` + `README.md`）
- 使用文本提示词生成3D结果（视频，必要时可导出mesh）。
- 在完整运行过程中监控GPU显存（`gpustat`）。
- 识别并分析至少2种失效案例。
- 仅调整1个超参数并进行对比分析。
- 准备不少于2页的ACM LaTeX报告材料。

## 3. 实验计划

### 当前环境快照（2026-03-07，Windows）
- Python：`3.13.5`
- PyTorch：`2.9.1+cu126`
- torchvision：`0.24.1+cu126`
- torchaudio：`2.9.1+cu126`
- `torch.cuda.is_available()`：`True`
- GPU设备数：`1`
- `nvcc`：当前未在PATH中识别（需按需补充CUDA Toolkit路径）
- 运行约束：仅使用项目路径下 `.venv`，不使用 conda

### 当前环境快照（2026-03-07，Linux，conda）
- OS：Linux（bash）
- conda env：`mtn-py39`
- Python：`3.9.25`
- GPU：NVIDIA GeForce RTX 3090（24GB）
- Driver：`570.211.01`（`nvidia-smi` 显示 CUDA Version: `12.8`）
- PyTorch：`1.13.1+cu117`（`torch.version.cuda == 11.7`）
- `torch.cuda.is_available()`：`True`
- CUDA扩展 import：`gridencoder/freqencoder/raymarching/shencoder` 均可 import

#### gpustat（Linux）
为保证在 conda env 下可复现，统一用 env 的 python 调用：

```bash
/home/ubuntu-user/anaconda3/envs/mtn-py39/bin/python -m pip install gpustat==1.1.1
/home/ubuntu-user/anaconda3/envs/mtn-py39/bin/python -m gpustat
# 训练过程中每秒刷新一次：
/home/ubuntu-user/anaconda3/envs/mtn-py39/bin/python -m gpustat -i 1
```

### `.venv` 兼容环境重建（Windows，不使用conda）
- 脚本路径：`scripts/rebuild_venv_windows.ps1`
- 推荐执行（重建 `.venv` 为 Python 3.10，并安装 PyTorch 1.13.1+cu117）：

```powershell
powershell -ExecutionPolicy Bypass -File .\scripts\rebuild_venv_windows.ps1 -PyVersion 3.10 -Recreate
```

- 在 `.venv` 内继续安装项目依赖：

```powershell
powershell -ExecutionPolicy Bypass -File .\scripts\rebuild_venv_windows.ps1 -PyVersion 3.10 -InstallRequirements
```

- 安装并验证 `gpustat`：

```powershell
powershell -ExecutionPolicy Bypass -File .\scripts\rebuild_venv_windows.ps1 -PyVersion 3.10 -InstallGpustat
```

### 阶段A一键检查脚本（Windows）
- 脚本路径：`scripts/phase_a_windows.ps1`
- 仅检查环境（推荐先执行）：

```powershell
powershell -ExecutionPolicy Bypass -File .\scripts\phase_a_windows.ps1
```

- 检查并安装 `gpustat`：

```powershell
powershell -ExecutionPolicy Bypass -File .\scripts\phase_a_windows.ps1 -InstallGpustat
```

- 检查并安装 `requirements.txt` + `gpustat`：

```powershell
powershell -ExecutionPolicy Bypass -File .\scripts\phase_a_windows.ps1 -InstallRequirements -InstallGpustat
```

### 阶段A：环境检查
- [ ] 检查CUDA工具链：`nvcc -V`
- [ ] 确认Python/PyTorch/CUDA版本兼容。
- [ ] 在 `.venv`（Python 3.10）中安装依赖：`.\.venv\Scripts\python.exe -m pip install -r requirements.txt --no-build-isolation`
- [ ] 在 `.venv` 中安装监控工具：`.\.venv\Scripts\python.exe -m pip install gpustat`
- [ ] 验证 `gpustat` 可正常运行。

### 阶段B：基线生成（任务1 + 任务2）
- 提示词候选：`a DSLR photo of a tiger dressed as a doctor`
- 基线命令（README中的 IF + perpneg 示例）：

```bash
python main.py -O --text "a DSLR photo of a tiger dressed as a doctor" --workspace trial_perpneg_if_tiger --iters 6000 --IF --batch_size 1 --perpneg --negative_w -3.0
```

- [ ] 记录实际执行的完整命令。
- [ ] 记录开始时间、结束时间、总耗时。
- [ ] 记录运行期间报错和警告。
- [ ] 记录输出产物路径（视频、日志、checkpoint）。
- [ ] 执行测试/导出命令：

```bash
python main.py --workspace trial_perpneg_if_tiger -O --test
```

### 阶段C：失效案例分析（任务3）
- [ ] 选择至少2种失效类型：
  - 几何畸变
  - 部件缺失
  - 纹理伪影
  - 形状失真
  - 多视角不一致
- [ ] 为每个失效案例保存截图。
- [ ] 记录失效出现的位置（阶段/视角/时间点）。

### 阶段D：单超参数实验（任务4）
- 目标文件与区域：`main.py`（按要求关注约22-173行的超参数区域）。
- 原则：一次只改一个超参数。
- 候选参数：
  - 学习率（`lr`）：例如默认值调整为 `3e-4` 或 `5e-4`（README建议）
- [ ] 在仅改一个变量的条件下重新运行。
- [ ] 与基线对比质量、稳定性、显存与耗时。

### 阶段E：报告整理
- [ ] 整理命令历史。
- [ ] 整理GPU显存数据表。
- [ ] 插入结果图、失效图、对比图。
- [ ] 完成不少于2页ACM LaTeX报告。
- [ ] 备份PDF、日志、视频与checkpoint。

## 4. 命令记录

| 时间 | 目的 | 命令 | 结果 |
|---|---|---|---|
| 2026-03-07 | 阅读需求并制定计划 | 阅读 `xuqiu.md`、`README.md` | 已完成 |
| 2026-03-07 | 环境确认 | `Python 3.13.5 + torch 2.9.1+cu126 + CUDA可用` | 已完成 |
| 2026-03-07 | 改为 `.venv` 路线（不用conda） | `powershell -ExecutionPolicy Bypass -File .\scripts\rebuild_venv_windows.ps1 -PyVersion 3.10 -Recreate` | 已完成（`.venv` 已重建为 Python 3.10，旧环境已自动备份） |
| 2026-03-07 | （Linux）安装 gpustat（conda env: mtn-py39） | `/home/ubuntu-user/anaconda3/envs/mtn-py39/bin/python -m pip install gpustat==1.1.1` | 已完成 |
| 2026-03-07 | （Linux）运行 gpustat | `/home/ubuntu-user/anaconda3/envs/mtn-py39/bin/python -m gpustat` | 已完成（gpustat 1.1.1） |
| TBD | CUDA检查 | `nvcc -V` |  |
| TBD | 安装依赖 | `.\.venv\Scripts\python.exe -m pip install -r requirements.txt --no-build-isolation` |  |
| TBD | 安装显存监控 | `.\.venv\Scripts\python.exe -m pip install gpustat` |  |
| 2026-03-07 | IF 基线训练（perpneg, 6000 iters / 60 epochs） | `python main.py -O --text "a DSLR photo of a tiger dressed as a doctor" --workspace trial_perpneg_if_tiger_baseline_6000 --iters 6000 --IF --batch_size 1 --perpneg --negative_w -3.0 --vram_O --num_steps 32 --upsample_steps 16` | 已完成（60/60；评估epoch 50/60；测试导出完成；耗时 75.6801 min） |
| 2026-03-07 | IF 基线测试导出 | `python main.py --workspace trial_perpneg_if_tiger_baseline_6000 -O --test` | 已完成（保存至 `trial_perpneg_if_tiger_baseline_6000/results`） |
| 2026-03-07 | 超参数对比尝试：lr=3e-4（失败） | `PATH="$HOME/bin:$PATH" PYTORCH_CUDA_ALLOC_CONF=max_split_size_mb:128 /home/ubuntu-user/anaconda3/envs/mtn-py39/bin/python main.py -O --text "a DSLR photo of a tiger dressed as a doctor" --workspace trial_perpneg_if_tiger_lr3e4_6000 --iters 6000 --IF --batch_size 1 --perpneg --negative_w -3.0 --vram_O --num_steps 32 --upsample_steps 16 --lr 3e-4` | 首轮 Epoch 1 的 `update_extra_state()` 阶段 OOM（见 `train_trial_perpneg_if_tiger_lr3e4_6000.log`） |
| 2026-03-08 | 单超参数对比：update_extra_interval=128（重训完成，6000 iters / 60 epochs） | `PATH="$HOME/bin:$PATH" PYTORCH_CUDA_ALLOC_CONF=max_split_size_mb:64,garbage_collection_threshold:0.8 /home/ubuntu-user/anaconda3/envs/mtn-py39/bin/python main.py -O --text "a DSLR photo of a tiger dressed as a doctor" --workspace trial_perpneg_if_tiger_update128_6000 --iters 6000 --IF --batch_size 1 --perpneg --negative_w -3.0 --vram_O --num_steps 32 --upsample_steps 16 --update_extra_interval 128` | 已完成（60/60；评估epoch 10/20/30/40/50/60；测试导出完成；耗时 262.1880 min；训练日志：`train_trial_perpneg_if_tiger_update128_6000_retrain.log`；显存监控：`gpustat_update128_retrain.log`） |
| 2026-03-08 | 单超参数对比：update_extra_interval=64（新增第三组，6000 iters / 60 epochs） | `PATH="$HOME/bin:$PATH" PYTORCH_CUDA_ALLOC_CONF=max_split_size_mb:64,garbage_collection_threshold:0.8 /home/ubuntu-user/anaconda3/envs/mtn-py39/bin/python main.py -O --text "a DSLR photo of a tiger dressed as a doctor" --workspace trial_perpneg_if_tiger_update64_6000 --iters 6000 --IF --batch_size 1 --perpneg --negative_w -3.0 --vram_O --num_steps 32 --upsample_steps 16 --update_extra_interval 64` | 运行中（训练日志：`train_trial_perpneg_if_tiger_update64_6000.log`；显存监控：`gpustat_update64_6000.log`；完成后补充耗时/结果路径/峰值显存） |

## 5. GPU显存记录（`gpustat`）

| 时间戳 | 阶段 | GPU利用率 | 已用显存 | 剩余显存 | 备注 |
|---|---|---|---|---|---|
| 2026-03-07 22:13:51 | 训练前（gpustat开始） | 25% | 374 MB | 24202 MB | driver 570.211.01；GPU0: RTX 3090 |
| 2026-03-07 22:14:51 | 训练+1分钟（采样） | 33% | 372 MB | 24204 MB | 仍在低负载阶段 |
| 2026-03-07 22:15:51 | 训练+2分钟（采样） | 26% | 390 MB | 24186 MB | 仍在低负载阶段 |
| 2026-03-07 22:16:51 | 训练+3分钟（采样） | 24% | 397 MB | 24179 MB | 仍在低负载阶段 |
| 2026-03-07 22:26:54 | 训练中期/高负载（采样） | 100% | 13914 MB | 10662 MB | 温度 81°C；训练进程约 13.6GB |
| 2026-03-07 23:36:43 | 训练结束（epoch 60/60 完成） | （见训练日志） | GPU=11.2GB（训练日志） |  | 训练日志记录：`Finished Epoch 60/60. CPU=14.3GB, GPU=11.2GB.` |
| 2026-03-07 23:41:?? | 测试导出阶段（结束） | （见训练日志） | （见训练日志） |  | 测试输出：`trial_perpneg_if_tiger_baseline_6000/results` |

update128（`update_extra_interval=128`）补充（重训）：

| 时间戳 | 阶段 | GPU利用率 | 已用显存 | 剩余显存 | 备注 |
|---|---|---|---|---|---|
| 2026-03-08（gpustat统计） | update128 重训全程（峰值） | （见 `gpustat_update128_retrain.log`） | 24105 MB | 471 MB | 由脚本扫描 `gpustat_update128_retrain.log` 得到峰值 |

补充：首次跑通 gpustat（记录任务要求信息）

| 时间戳 | 阶段 | GPU利用率 | 已用显存 | 剩余显存 | 备注 |
|---|---|---|---|---|---|
| 2026-03-07 19:29 | 环境验证（Linux） | 27% | 364 MB | 24576-364 MB | GPU0：RTX 3090；Driver 570.211.01；进程包含 VS Code 等图形进程 |

## 6. 输出产物记录

| 产物 | 路径 | 状态 | 备注 |
|---|---|---|---|
| 训练日志 | `train_trial_perpneg_if_tiger_baseline_6000.log` | 已生成 | 含epoch进度、eval(50/60)、测试导出与总耗时（75.6801 min） |
| GPU监控日志（gpustat） | `gpustat_baseline.log` | 已生成 | 训练期间显存与GPU利用率采样依据 |
| checkpoint | `trial_perpneg_if_tiger_baseline_6000/checkpoints/` | 已生成 | （目录存在；如需可补充文件名列表） |
| 360视频 / 渲染结果 | `trial_perpneg_if_tiger_baseline_6000/results/` | 已生成 | 训练结束后执行 Test 阶段生成 |
| 360视频 / 渲染结果（update128） | `trial_perpneg_if_tiger_update128_6000/results/` | 已生成 | 通过 `python main.py --workspace trial_perpneg_if_tiger_update128_6000 -O --test` 导出（见 `test_trial_perpneg_if_tiger_update128_6000.log`） |
| mesh（可选） | `workspace/mesh/...` |  |  |
| 失效截图1 | `assets/failure_1.png` |  |  |
| 失效截图2 | `assets/failure_2.png` |  |  |

## 7. 失效案例记录

### 失效案例1
- 类型：
- 证据截图文件：
- 出现位置：
- 现象描述：
- 可能原因：
- 对最终质量的影响：

### 失效案例2
- 类型：
- 证据截图文件：
- 出现位置：
- 现象描述：
- 可能原因：
- 对最终质量的影响：

## 8. 超参数敏感性记录

- 调整参数：`update_extra_interval`
- 原始值：`16`
- 新值：`128`
- 选择该参数的原因：`update_extra_state()` 会周期性触发额外的网格/密度状态更新；在 `lr=3e-4` 尝试中，训练刚开始（Epoch 1 / step 0）就在该阶段发生 OOM。
  将 interval 从 16 提高到 128 可以显著降低该高峰值操作的触发频率，从而提升训练稳定性（以不改模型结构/分辨率为前提）。

补充说明：
- `lr=3e-4` 尝试记录：`train_trial_perpneg_if_tiger_lr3e4_6000.log`（失败于 `axisnetworks.py` 的 sin/cos embedding，OOM 申请 128MiB）。
- `update128` 对比记录：`train_trial_perpneg_if_tiger_update128_6000.log` + `gpustat_update128.log`（完成后补充耗时、峰值显存、结果路径与对比结论）。
- 当前状态更新（2026-03-08）：
  - 训练未完成：checkpoint 最新为 `df_ep0056.pth`（test log 显示 `load at epoch 56, global step 5600`）。
  - 已完成 test 导出：输出到 `trial_perpneg_if_tiger_update128_6000/results`。
  - 峰值显存（gpustat统计）：24103MB/24576MB。

### 对比总结
| 指标 | 基线 | 调整后 | 观察结论 |
|---|---|---|---|
| 视觉质量 | 已生成（`trial_perpneg_if_tiger_baseline_6000/results/`） | 已生成（`trial_perpneg_if_tiger_update128_6000/results/`） | 两者均已产出可用于对比的视频/渲染结果；update128 的训练未跑满 60 epoch，因此“训练质量”的严格可比性不足，但 test 导出结果可用于初步观察。 |
| 稳定性（坍塌/闪烁/伪影） | 训练完整跑到 60/60（log 含 `Evaluate epoch 60 Finished` + `Finished Test`） | 训练中断在 epoch 56->57 之间（仅看到 `Start Epoch 57/60`，无 `Finished Epoch 57/60`）；但 `--test` 可正常加载 `df_ep0056.pth` 并完成导出 | update128 参数本身未触发早期 OOM（对比 lr=3e-4 的失败）；当前不稳定来自训练未完成/中断，而非已观察到的训练过程崩溃。 |
| 运行耗时 | `training takes 75.6801 minutes.`（见 `train_trial_perpneg_if_tiger_baseline_6000.log`） | 暂无完整训练耗时（训练未完成）；test 导出耗时约 20s（见 `test_trial_perpneg_if_tiger_update128_6000.log`） | 目前只能对比“可导出”与“峰值显存”；若需要严格对比耗时，必须将 update128 续训/重训至 60/60。 |
| 峰值GPU显存 | 训练结束时 log 记录 GPU=11.2GB（`Finished Epoch 60/60... GPU=11.2GB`） | `gpustat_update128.log` 统计峰值 24103MB/24576MB（约 24.1GB） | update128 训练阶段显存峰值显著更高（接近满显存），但仍能在未 OOM 的情况下推进到 epoch 56。 |
| 失效严重程度 | TBD（需从结果视频截图记录 2 个失效案例） | TBD（需从结果视频截图记录 2 个失效案例） | 下一步应从 `results/` 中截取至少 2 类失效（几何畸变/纹理伪影/多视角不一致等），分别填入第 7 节。 |

## 9. 问题排查记录

| 时间 | 问题 | 处理动作 | 结果 |
|---|---|---|---|
| TBD |  |  |  |

常见修复思路（来自需求与README）：
- OOM：降低分辨率（64 -> 48）、减小batch size、关闭其他GPU进程。
- 结果不稳定：尝试更换提示词/随机种子，调整学习率（`3e-4` 或 `5e-4`）。
- 构建或版本不匹配：依据 `nvcc -V` 和 PyTorch兼容矩阵对齐版本。

## 10. 备份检查清单
- [ ] 重训前备份checkpoint。
- [ ] 备份生成视频与截图。
- [ ] 备份日志与命令记录。
- [ ] 备份LaTeX源文件与最终PDF。

PowerShell示例（备份checkpoint）：
```powershell
Copy-Item .\trial\checkpoints\df_ep0060.pth .\trial\checkpoints\df_ep0060_backup.pth
```

## 11. 当前阻塞与下一步（`.venv`）
- 当前阻塞：`torch 1.13.1+cu117` 安装在下载/安装阶段中断，尚未完成。
- 立即下一步：在项目根目录继续执行以下命令直到成功。

```powershell
.\.venv\Scripts\python.exe -m pip install torch==1.13.1+cu117 torchvision==0.14.1+cu117 torchaudio==0.13.1 --extra-index-url https://download.pytorch.org/whl/cu117
```

- 安装完成后验证：

```powershell
.\.venv\Scripts\python.exe -c "import torch; print(torch.__version__); print(torch.version.cuda); print(torch.cuda.is_available())"
```

- 再继续安装项目依赖与监控工具：

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements.txt --no-build-isolation
.\.venv\Scripts\python.exe -m pip install gpustat
.\.venv\Scripts\python.exe -m gpustat
```
