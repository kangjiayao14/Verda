# 青野 Verda · AI 竞品情报工作台

> 让每个结论都有出处，让每次调研都活着。

一个会自己组队、能溯源、看得见思考过程的「AI 竞品情报工作台」。

- **前端**：React 18 + TypeScript + Vite + TailwindCSS + Zustand + Framer Motion + ECharts / React Flow
- **后端**：FastAPI + LangGraph + SQLite
- **LLM**：豆包 Doubao-Seed-2.0-lite（火山方舟 / Ark OpenAI 兼容网关）

## 目录结构

```
.
├── frontend/             # React + Vite 前端
│   ├── src/
│   │   ├── components/   # 通用组件（V 前缀组件库）
│   │   ├── layout/       # AppLayout / VSidebar 全局框架
│   │   ├── pages/        # 8 个页面
│   │   ├── store/        # Zustand 状态
│   │   ├── hooks/        # 自定义 hooks（含 SSE）
│   │   └── lib/api.ts    # 后端 API 封装
│   └── public/assets/    # 静态资源（头像、品牌图等）
├── backend/              # FastAPI + LangGraph 后端
│   ├── app/
│   │   ├── core/         # config / llm / search / fetcher / orchestrator / db ...
│   │   ├── data/         # 专家清单等静态数据
│   │   └── main.py       # FastAPI 入口
│   ├── requirements.txt
│   └── .env.example      # 环境变量模板（密钥走环境变量，不硬编码）
├── .gitignore
└── README.md
```

## 环境要求

- Node.js ≥ 18（推荐 22.x）
- Python ≥ 3.9

## 快速开始

### 1. 克隆仓库

```bash
git clone <your-repo-url>
cd <repo>
```

### 2. 配置后端密钥

```bash
cd backend
cp .env.example .env
# 编辑 .env，填入自己的 ARK_API_KEY / SERPAPI_KEY 等
```

| 变量 | 说明 | 必填 |
|---|---|---|
| `ARK_API_KEY` | 火山方舟 API Key（豆包） | 是（LLM 真实调用） |
| `DOUBAO_ENDPOINT_ID` | 推理接入点 ID | 是 |
| `SERPAPI_KEY` / `BING_SEARCH_KEY` | 搜索 API（采集 Agent） | 采集真实联网时需要 |
| `DOUYIN_COOKIE` / `BILIBILI_COOKIE` / `XHS_COOKIE` | 各平台采集 cookie | 平台采集时需要 |

> ⚠️ `.env` 已在 `.gitignore` 中，**严禁**把真实密钥提交到 GitHub。

### 3. 启动后端

```bash
cd backend
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
.venv/bin/python -m uvicorn app.main:app --reload --port 8000
# http://localhost:8000
```

验证 LLM 是否打通：

```bash
curl http://localhost:8000/api/llm/ping
curl http://localhost:8000/health
```

### 4. 启动前端

```bash
cd frontend
npm install
npm run dev
# http://localhost:5173
```

如需自定义后端地址，可在 `frontend/.env.local` 中设置：

```
VITE_API_BASE=http://localhost:8000
```

## 安全提示

- 仓库中的 `backend/.env.example` 仅含占位符，请勿在该文件填入真实密钥后提交。
- `backend/.env`、`*.env`、`.venv/`、`node_modules/`、`dist/`、运行时 SQLite 等已通过 `.gitignore` 忽略。
- 若误提交了密钥，请立刻在对应平台吊销 / 轮换，并清理 git 历史。

## License

仅作个人作品 / 学习用途。
