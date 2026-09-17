# Jetson AGX Orin + JetPack 5 + Ubuntu 20.04
# Codex CLI 串接校內 iAI 模型設定流程

---

# 1. 環境架構

目標：

- NVIDIA Jetson AGX Orin 64GB
- JetPack 5.x
- Ubuntu 20.04
- ROS2
- MoveIt2
- Codex CLI
- CC Switch
- 校內 iAI API


架構：
```

Jetson AGX Orin

Ubuntu 20.04

│\
├── ROS2\
│\
├── MoveIt2\
│\
├── OpenCV\
│\
├── TensorRT\
│\
├── CM530 ROS Bridge\
│\
├── AX-12A\
│\
└── Codex CLI\
|\
|\
CC Switch\
|\
|\
iAI API\
|\
|\
校內模型
````yaml


---

# 2. 確認 Jetson 環境

## 2.1 查看 Ubuntu 版本

```bash
lsb_release -a
````

正常：
```yaml
Distributor ID: Ubuntu
Description: Ubuntu 20.04 LTS
```

---

## 2.2 查看 JetPack / L4T版本
```bash
cat /etc/nv_tegra_release
```

範例：
```
# R35 (release), REVISION: 5.1
```

代表：
```
JetPack 5.x
Jetson Linux R35.x
Ubuntu 20.04
```

NVIDIA JetPack 5.x 使用 Ubuntu 20.04 based root filesystem。

參考：

[https://docs.nvidia.com/jetson/jetpack/5.1/install-jetpack/index.html](https://docs.nvidia.com/jetson/jetpack/5.1/install-jetpack/index.html)

---

# 3. 確認 Codex CLI

查看版本：
```bash
codex --version
```

成功：
```
codex-cli x.x.x
```

---

# 4. 安裝 CC Switch

官方：

[https://ccswitch.co/zh/](https://ccswitch.co/zh/)

下載：
```
CC-Switch.AppImage
```

---

## 4.1 增加執行權限

進入下載位置：
```bash
cd ~/Downloads
```

設定：
```bash
chmod +x CC-Switch*.AppImage
```

執行：
```bash
./CC-Switch*.AppImage
```

---

# 5. Ubuntu 20.04 相容問題

若出現：
```
GLIBC_x.xx not found
```

表示 CC Switch 新版與 Ubuntu 20.04 不相容。

若：
```
FUSE error
```

安裝：
```bash
sudo apt update

sudo apt install libfuse2
```

重新執行：
```bash
./CC-Switch*.AppImage
```

---

# 6. CC Switch 設定 iAI

## Step 1

開啟 CC Switch

選：
```
Codex
```

點：
```
新增
```

---

## Step 2

建立 Provider

名稱：
```
iAI
```

---

## Step 3

填寫 API

API Key：
```
填入學校申請 API KEY
```

API URL：
```
https://www.iai.nkust.edu.tw/aihub/v1
```

模式：
```
Chat Completions
```

---

# 7. 取得模型

按：
```
取得模型清單
```

成功：
```
取得 X 個模型
```

---

# 8. 新增模型映射

流程：
```
新增模型

↓

選擇使用模型

↓

填寫模型名稱

↓

新增
```

例如：

模型名稱：
```
iAI-Codex
```

模型 ID：
```
選擇取得列表中的模型
```

---

# 9. 開啟 CC Switch 路由

進入：
```
設定

↓

路由
```

開啟：
```
本地路由
```

確認：
```
Codex = ON
```

---

# 10. 測試 Codex

重新開 Terminal：
```bash
codex
```

輸入：
```
你好
```

成功流程：
```
Codex CLI

↓

CC Switch Local Router

↓

iAI API

↓

校內模型
```

---

# 11. 不使用 CC Switch 備案

如果 Jetson 無法執行 CC Switch GUI：

直接設定 API。

編輯：
```bash
nano ~/.bashrc
```

加入：
```bash
export OPENAI_BASE_URL=https://www.iai.nkust.edu.tw/aihub/v1

export OPENAI_API_KEY=你的API_KEY
```

更新：
```bash
source ~/.bashrc
```

測試：
```bash
codex
```

---

# 12. Jetson 機械手臂專案架構
```
Camera

↓

OpenCV

↓

AI Model

↓

ROS2 Topic

↓

MoveIt2

↓

CM530 ROS Bridge

↓

AX-12A
```

Codex用途：

- ROS2 Node 開發
- Python / C++ 撰寫
- Debug
- 演算法協助

AI模型用途：

- YOLO
- TensorRT
- OpenCV
- Vision AI

---

# 13. 注意事項

Jetson AGX Orin主要負責：

- GPU AI推論
- ROS2控制
- TensorRT加速
- 機器人控制

不要因為安裝 AI 工具破壞：

- CUDA
- cuDNN
- TensorRT
- ROS2環境

建議：
```
Jetson AGX Orin

負責：
ROS2
AI inference
Robot control


Laptop

負責：
CC Switch
文件
程式管理
Git
```

---

# References

NVIDIA JetPack Documentation:

[https://docs.nvidia.com/jetson/jetpack/](https://docs.nvidia.com/jetson/jetpack/)

CC Switch:

[https://ccswitch.co/zh/](https://ccswitch.co/zh/)

校內 iAI 串接流程：

Codex 串接 AI 神器教學 PDF
```yaml

---

這份版本可以直接放進你的 **Jetson AGX Orin / CM530 專案 GitHub README 或 docs 資料夾**。  
另外提醒：你提供的 PDF 裡 iAI URL、Chat Completions、本地路由、Codex 開關流程已整合進去；CC Switch 對 Codex 使用 Chat Completions 類自訂 Provider 時，需要透過本地路由做協定轉換，這也是你這個案例的關鍵。:contentReference[oaicite:0]{index=0} :contentReference[oaicite:1]{index=1}
