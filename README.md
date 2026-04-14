# 法衡·公评 | JustJudge

> 裁判文书智能评价系统 - Judge Document Intelligent Evaluation System

## 项目介绍

**法衡·公评（JustJudge）** 是一套面向中文裁判文书的智能评价系统，融合多智能体评审、事件图谱、技能系统和联网检索等先进技术，为法官和法律从业者提供专业、客观的裁判文书质量评价服务。

### 核心特性

- **多智能体协同评审**：4个专业评审智能体（格式规范、事实证据、法律说理、程序合规）+ 首席仲裁智能体
- **事件图谱驱动**：构建案件事实与法律关系的结构化表示
- **联网检索能力**：实时检索法律条文和参考案例
- **技能系统**：可复用的专业技能组件（格式检查、错别字检查、法条检索等）
- **定量定性结合**：多维评分 + 详细问题发现 + 改进建议

## 系统架构

```
┌─────────────────────────────────────────────────────────────────┐
│                        裁判文书智能评价系统                       │
├─────────────────────────────────────────────────────────────────┤
│  ┌──────────┐    ┌──────────┐    ┌──────────┐                  │
│  │  输入层   │───▶│  解析层   │───▶│  图谱层   │                  │
│  │ 文书文本  │    │ 结构化抽取 │    │ 事件图谱  │                  │
│  └──────────┘    └──────────┘    └──────────┘                  │
│        │              │              │                          │
│        ▼              ▼              ▼                          │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                   多智能体评审层                         │   │
│  │  ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐          │   │
│  │  │Agent_F │ │Agent_FE│ │Agent_LR│ │Agent_P │          │   │
│  │  │格式规范│ │事实证据│ │法律说理│ │程序合规│          │   │
│  │  └────────┘ └────────┘ └────────┘ └────────┘          │   │
│  │                         │                             │   │
│  │                         ▼                             │   │
│  │              ┌──────────────────┐                     │   │
│  │              │  Chief Judge     │                     │   │
│  │              │  首席仲裁智能体   │                     │   │
│  │              └──────────────────┘                     │   │
│  └─────────────────────────────────────────────────────────┘   │
│                          │                                      │
│                          ▼                                      │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                    评价生成与展示层                       │   │
│  └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

## 技术栈

### 后端
- **框架**：FastAPI
- **Python**: 3.11+
- **LLM**: OpenAI API 兼容接口 (Kimi-K2.5)
- **向量数据库**: ChromaDB
- **图数据库**: NetworkX
- **文档解析**: Python-docx, PyPDF2

### 前端
- **框架**：Next.js 14
- **UI 库**：React 18, Tailwind CSS
- **可视化**：Recharts, Vis-Network
- **图标**：Lucide React

### 基础设施
- **Docker & Docker Compose**
- **SearXNG**: 搜索后端 (端口 8080)

## 快速开始

### 1. 克隆仓库

```bash
git clone <repository-url>
cd justjudge
```

### 2. 配置环境变量

```bash
cp .env.example .env
# 编辑 .env 文件，配置必要的参数
```

### 3. 使用 Docker Compose 启动

```bash
# 一键启动所有服务
docker-compose up -d

# 查看日志
docker-compose logs -f

# 停止服务
docker-compose down
```

### 4. 手动安装和运行

#### 后端

```bash
cd backend
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

#### 前端

```bash
cd frontend
npm install
npm run dev
```

### 5. 访问系统

- 前端界面: http://localhost:9090
- 后端 API: http://localhost:8000
- API 文档: http://localhost:8000/docs

## 项目结构

```
justjudge/
├── backend/                    # Python FastAPI 后端
│   ├── app/
│   │   ├── main.py            # FastAPI 入口
│   │   ├── config.py          # 配置管理
│   │   ├── models/            # Pydantic 数据模型
│   │   ├── agents/            # 多智能体评审
│   │   ├── skills/            # Skills 技能系统
│   │   ├── graph/             # 事件图谱
│   │   ├── parser/            # 文书解析
│   │   ├── evaluation/        # 评价计算
│   │   ├── api/               # API 路由
│   │   └── utils/             # 工具函数
│   ├── requirements.txt
│   └── Dockerfile
├── frontend/                   # Next.js 前端
│   ├── src/
│   │   ├── app/               # Next.js App Router
│   │   ├── components/        # React 组件
│   │   ├── lib/               # 工具函数
│   │   └── styles/            # 样式文件
│   ├── package.json
│   └── Dockerfile
├── data/                      # 数据目录
├── docs/                      # 文档
├── config.yaml               # 系统配置
├── docker-compose.yml        # Docker 编排
├── start.sh                  # 启动脚本
└── README.md
```

## 评价维度

系统从以下四个维度对裁判文书进行评价：

### 1. 格式规范 (20-25%)
- 首部完整性
- 尾部规范性
- 结构完整性
- 语言文字质量
- 技术规范

### 2. 事实证据 (20-30%)
- 争议焦点归纳
- 诉辩意见归纳
- 证据列举完整性
- 举证质证反映
- 证据采信理由
- 事实认定逻辑

### 3. 法律说理 (30-40%)
- 争议焦点回应
- 法条引用准确性
- 法律解释合理性
- 论证逻辑性
- 说理充分性
- 案例参考

### 4. 程序合规 (15-25%)
- 受理程序
- 审理程序
- 当事人权利保障
- 审判组织
- 诉讼费用

## 质量等级

| 等级 | 分数范围 | 说明 |
|------|----------|------|
| 优秀 | 90-100 | 文书质量优秀，无明显问题 |
| 良好 | 75-89 | 文书质量良好，存在少量瑕疵 |
| 合格 | 60-74 | 文书质量合格，需要改进 |
| 不合格 | 0-59 | 存在重大错误，需要重写 |

## API 使用示例

```bash
# 上传裁判文书
curl -X POST "http://localhost:8000/api/v1/documents/upload" \
  -H "Content-Type: multipart/form-data" \
  -F "file=@judgment.txt"

# 启动评价
curl -X POST "http://localhost:8000/api/v1/evaluation/start" \
  -H "Content-Type: application/json" \
  -d '{"document_id": "doc-xxx", "level": "comprehensive"}'

# 获取评价结果
curl "http://localhost:8000/api/v1/evaluation/eval-xxx"
```

## 配置说明

主要配置文件：`config.yaml`

```yaml
# 智能体配置
agents:
  agent_f:     # 格式规范专家
  agent_fe:    # 事实证据专家
  agent_lr:    # 法律说理专家
  agent_p:     # 程序合规专家
  agent_chief: # 首席仲裁智能体

# 评分权重 (按案件类型)
scoring:
  dimension_weights:
    civil_first_instance:  # 民事一审
      format: 0.20
      fact_evidence: 0.25
      legal_reasoning: 0.30
      procedure: 0.25

# 联网搜索配置
web_search:
  enabled: true
  searxng_url: "http://localhost:8080"
```

## 开发指南

### 添加新的智能体

1. 继承 `BaseAgent` 类
2. 实现 `evaluate` 方法
3. 在 `config.yaml` 中配置权重

### 添加新的技能

1. 继承 `Skill` 类
2. 实现 `execute` 方法
3. 在 `SkillRegistry` 中注册

## 贡献指南

欢迎提交 Issue 和 Pull Request！

1. Fork 本项目
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 创建 Pull Request

## 许可证

[MIT](LICENSE)

## 联系方式

- 项目维护者：JustJudge Team
- 项目主页：https://github.com/your-org/justjudge

---

**法衡·公评** - 让裁判文书评价更专业、更客观、更高效
