<div align="center">

# AI影片轉錄器

繁體中文 | [English](README.md)

一款開源的AI影片轉錄和摘要工具，支持YouTube、Bilibili、抖音等30+平台。

![Interface](cn-video.png)

</div>

## ✨ 功能特性

- 🎥 **多平台支持**: 支持YouTube、Bilibili、抖音等30+平台。
- 🗣️ **智慧轉錄**: 使用Faster-Whisper模型進行高精度語音轉文字
- 🤖 **AI文字優化**: 自動錯別字修正、句子完整化和智慧分段
- 🌍 **多語言摘要**: 支持多種語言的智慧摘要生成
- ⚙️ **條件式翻譯**：當所選摘要語言與Whisper檢測到的語言不一致時，自動調用GPT‑4o生成翻譯
- 📱 **移動適配**: 完美支持移動設備

## 🚀 快速開始

### 環境要求

- Python 3.8+
- FFmpeg
- 可選：OpenAI API金鑰（用於智慧摘要功能）

### 安裝方法


#### 方法一：自動安裝

```bash
# 克隆專案
git clone https://github.com/wendy7756/AI-Video-Transcriber.git
cd AI-Video-Transcriber

# 運行安裝腳本
chmod +x install.sh
./install.sh
```

#### 方法二：Docker部署

```bash
# 克隆專案
git clone https://github.com/wendy7756/AI-Video-Transcriber.git
cd AI-Video-Transcriber

# 複製環境變數模板
cp .env.example .env
# 編輯.env文件，設置你的OPENAI_API_KEY

# 🚀 推薦：使用優化構建腳本（自動啟用 BuildKit 加速）
./build-docker.sh

# 或者手動使用 Docker Compose
# CPU模式（推薦，大多數用戶）
docker-compose up -d

# GPU模式（需要NVIDIA GPU）
docker-compose --profile gpu up -d

# 或者直接使用Docker
# CPU版本
docker build -t ai-video-transcriber .
docker run -p 8893:8893 --env-file .env ai-video-transcriber

# GPU版本（需要NVIDIA Docker支援）
docker build -f Dockerfile.gpu -t ai-video-transcriber-gpu .
docker run --gpus all -p 8893:8893 --env-file .env ai-video-transcriber-gpu
```

**🚀 加速提示：**
- 使用 `./build-docker.sh` 腳本自動啟用 BuildKit 加速
- 首次構建可能需要 5-10 分鐘，之後的重建會快很多
- 如果網路慢，可以考慮使用 VPN 或更換網路環境

#### 方法三：手動安裝

1. **安裝Python依賴**（建議使用虛擬環境）
```bash
# 創建並啟用虛擬環境（macOS推薦，避免 PEP 668 系統限制）
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```

2. **安裝FFmpeg**
```bash
# macOS
brew install ffmpeg

# Ubuntu/Debian
sudo apt update && sudo apt install ffmpeg

# CentOS/RHEL
sudo yum install ffmpeg
```

3. **配置環境變數**（摘要/翻譯功能需要）
```bash
# 必需：啟用智慧摘要/翻譯
export OPENAI_API_KEY="你的_API_Key"

# 可選：如使用自建/代理的OpenAI兼容網關，按需設置
```

### 啟動服務

```bash
python3 start.py
```

服務啟動後，打開瀏覽器訪問 `http://localhost:8893`

#### 使用顯式環境變數啟動（示例）

```bash
source .venv/bin/activate
export OPENAI_API_KEY=你的_API_Key
# export OPENAI_BASE_URL=https://oneapi.basevec.com/v1  # 如使用自定義端點
python3 start.py --prod
```

## 📖 使用指南

1. **輸入影片鏈接**: 在輸入框中粘貼YouTube、Bilibili等平台的影片鏈接
2. **選擇摘要語言**: 選擇希望生成摘要的語言
3. **開始處理**: 點擊"開始"按鈕
4. **監控進度**: 觀察實時處理進度，包含多個階段：
   - 影片下載和解析
   - 使用Faster-Whisper進行音頻轉錄
   - AI智慧轉錄優化（錯別字修正、句子完整化、智慧分段）
   - 生成選定語言的AI摘要
5. **查看結果**: 查看優化後的轉錄文本和智慧摘要
6. **下載文件**: 點擊下載按鈕保存Markdown格式的文件

## 🛠️ 技術架構

### 後端技術棧
- **FastAPI**: 現代化的Python Web框架
- **yt-dlp**: 影片下載和處理
- **Faster-Whisper**: 高效的語音轉錄
- **OpenAI API**: 智慧文字摘要

### 前端技術棧
- **HTML5 + CSS3**: 響應式介面設計
- **JavaScript (ES6+)**: 現代化的前端交互
- **Marked.js**: Markdown渲染
- **Font Awesome**: 圖標庫

### 專案結構
```
AI-Video-Transcriber/
├── backend/                 # 後端代碼
│   ├── main.py             # FastAPI主應用
│   ├── video_processor.py  # 影片處理模組
│   ├── transcriber.py      # 轉錄模組
│   ├── summarizer.py       # 摘要模組
│   └── translator.py       # 翻譯模組
├── static/                 # 前端文件
│   ├── index.html          # 主頁面
│   └── app.js              # 前端邏輯
├── temp/                   # 臨時文件目錄
├── Docker相關文件           # Docker部署
│   ├── Dockerfile          # Docker鏡像配置
│   ├── docker-compose.yml  # Docker Compose配置
│   └── .dockerignore       # Docker忽略規則
├── .env.example        # 環境變數模板
├── requirements.txt    # Python依賴
└── start.py           # 啟動腳本

```

## ⚙️ 配置選項

### 環境變數

| 變數名 | 描述 | 預設值 | 必需 |
|--------|------|--------|------|
| `OPENAI_API_KEY` | OpenAI API金鑰 | - | 否 |
| `HOST` | 伺服器地址 | `0.0.0.0` | 否 |
| `PORT` | 伺服器端口 | `8893` | 否 |
| `WHISPER_MODEL_SIZE` | Whisper模型大小 | `base` | 否 |

### Whisper模型大小選項

| 模型 | 參數量 | 英語專用 | 多語言 | 速度 | 記憶體占用 |
|------|--------|----------|--------|------|----------|
| tiny | 39 M | ✓ | ✓ | 快 | 低 |
| base | 74 M | ✓ | ✓ | 中 | 低 |
| small | 244 M | ✓ | ✓ | 中 | 中 |
| medium | 769 M | ✓ | ✓ | 慢 | 中 |
| large | 1550 M | ✗ | ✓ | 很慢 | 高 |

## 🔧 常見問題

### Q: 為什麼轉錄速度很慢？
A: 轉錄速度取決於影片長度、Whisper模型大小和硬體性能。可以嘗試使用更小的模型（如tiny或base）來提高速度。

### Q: 支持哪些影片平台？
A: 支持所有yt-dlp支持的平台，包括但不限於：YouTube、抖音、Bilibili、優酷、愛奇藝、騰訊影片等。

### Q: AI優化功能不可用怎麼辦？
A: 轉錄優化和摘要生成都需要OpenAI API金鑰。如果未配置，系統會提供Whisper的原始轉錄和簡化版摘要。

### Q: 出現 500 報錯/白屏，是代碼問題嗎？
A: 大多數情況下是環境配置問題，請按以下清單排查：
- 是否已啟動虛擬環境：`source .venv/bin/activate`
- 依賴是否安裝在虛擬環境中：`pip install -r requirements.txt`
- 是否設置 `OPENAI_API_KEY`（啟用摘要/翻譯所必需）
- 如使用自定義網關，`OPENAI_BASE_URL` 是否正確、網路可達
- 是否已安裝 FFmpeg：macOS `brew install ffmpeg` / Debian/Ubuntu `sudo apt install ffmpeg`
- 8893 端口是否被占用；如被占用請關閉舊進程或更換端口

### Q: 如何處理長影片？
A: 系統可以處理任意長度的影片，但處理時間會相應增加。建議對於超長影片使用較小的Whisper模型。

### Q: 如何使用Docker部署？
A: Docker提供了最簡單的部署方式：

**前置條件：**
- 從 https://www.docker.com/products/docker-desktop/ 安裝Docker Desktop
- 確保Docker服務正在運行

**快速開始：**
```bash
# 克隆和配置
git clone https://github.com/wendy7756/AI-Video-Transcriber.git
cd AI-Video-Transcriber
cp .env.example .env
# 編輯.env文件設置你的OPENAI_API_KEY

# 使用Docker Compose啟動（推薦）
docker-compose up -d

# 或手動構建運行
docker build -t ai-video-transcriber .
docker run -p 8893:8893 --env-file .env ai-video-transcriber
```

**常見Docker問題：**
- **端口衝突**：如果8893端口被占用，可改用 `-p 8894:8893`
- **權限拒絕**：確保Docker Desktop正在運行且有適當權限
- **構建失敗**：檢查磁碟空間（需要約2GB空閒空間）和網路連接
- **容器無法啟動**：驗證.env文件存在且包含有效的OPENAI_API_KEY

**Docker常用命令：**
```bash
# 查看運行中的容器
docker ps

# 檢查容器日誌
docker logs ai-video-transcriber-ai-video-transcriber-1

# 停止服務
docker-compose down

# 修改後重新構建
docker-compose build --no-cache
```

### Q: 記憶體需求是多少？
A: 記憶體使用量根據部署方式和工作負載而有所不同：

**Docker部署：**
- **基礎記憶體**：空閒容器約128MB
- **處理過程中**：根據影片長度和Whisper模型，需要500MB - 2GB
- **Docker鏡像大小**：約1.6GB磁碟空間
- **推薦配置**：4GB+記憶體以確保流暢運行

**傳統部署：**
- **基礎記憶體**：FastAPI伺服器約50-100MB
- **Whisper模型記憶體占用**：
  - `tiny`：約150MB
  - `base`：約250MB
  - `small`：約750MB
  - `medium`：約1.5GB
  - `large`：約3GB
- **峰值使用**：基礎 + 模型 + 影片處理（額外約500MB）

**記憶體優化建議：**
```bash
# 使用更小的Whisper模型減少記憶體占用
WHISPER_MODEL_SIZE=tiny  # 或 base

# Docker部署時可限制容器記憶體
docker run -m 1g -p 8893:8893 --env-file .env ai-video-transcriber

# 監控記憶體使用情況
docker stats ai-video-transcriber-ai-video-transcriber-1
```

### Q: 網路連接錯誤或超時怎麼辦？
A: 如果在影片下載或API調用過程中遇到網路相關錯誤，請嘗試以下解決方案：

**常見網路問題：**
- 影片下載失敗，出現"無法提取"或超時錯誤
- OpenAI API調用返回連接超時或DNS解析失敗
- Docker鏡像拉取失敗或極其緩慢

**解決方案：**
1. **切換VPN/代理**：嘗試連接到不同的VPN伺服器或更換代理設置
2. **檢查網路穩定性**：確保你的網路連接穩定
3. **更換網路後重試**：更改網路設置後等待30-60秒再重試
4. **使用備用端點**：如果使用自定義OpenAI端點，驗證它們在你的網路環境下可訪問
5. **Docker網路問題**：如果容器網路失敗，重啟Docker Desktop

**快速網路測試：**
```bash
# 測試影片平台訪問
curl -I https://www.youtube.com/

# 測試OpenAI API訪問（替換為你的端點）
curl -I https://api.openai.com

# 測試Docker Hub訪問
docker pull hello-world
```

如果問題持續存在，嘗試切換到不同的網路或VPN位置。

## 🤝 貢獻指南

歡迎提交Issue和Pull Request！

1. Fork專案
2. 創建功能分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 開啟Pull Request 

## 致謝

- [yt-dlp](https://github.com/yt-dlp/yt-dlp) - 強大的影片下載工具
- [Faster-Whisper](https://github.com/guillaumekln/faster-whisper) - 高效的Whisper實現
- [FastAPI](https://fastapi.tiangolo.com/) - 現代化的Python Web框架
- [OpenAI](https://openai.com/) - 智慧文字處理API

## 📞 聯絡方式

如有問題或建議，請提交Issue或聯絡Wendy。
