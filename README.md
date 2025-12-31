# 高校大型仪器设备共享服务平台

一个基于 Flask + Vue 3 的前后端分离的高校仪器设备预约管理系统，为师生提供便捷、高效的仪器设备预约服务。

## 项目简介

此为HNU数据库系统大作业项目，由七人小组共同开发。在未来希望将本系统开发为一个完整的仪器设备预约管理平台，支持学生、教师和管理员三种角色，提供实验室管理、设备管理、预约管理、用户管理等功能。系统采用前后端分离架构，后端提供 RESTful API，前端使用现代化的 Vue 3 框架构建用户界面。

## 技术栈

### 后端

- **Web 框架**: Flask 3.0.0
- **ORM**: Flask-SQLAlchemy 3.1.1
- **数据库迁移**: Flask-Migrate 4.0.5
- **序列化**: Marshmallow 3.20.1 / Flask-Marshmallow 0.15.0
- **API 文档**: Flasgger 0.9.7.1 (Swagger UI)
- **数据库**: MySQL (TiDB Cloud Serverless)
- **数据库驱动**: PyMySQL 1.1.0
- **环境变量管理**: python-dotenv 1.0.0

### 前端

- **框架**: Vue 3.4.15
- **路由**: Vue Router 4.2.5
- **状态管理**: Pinia 2.1.7
- **UI 组件库**: Element Plus 2.5.6
- **HTTP 客户端**: Axios 1.6.5
- **构建工具**: Vite 5.0.11
- **样式预处理**: Sass 1.69.5
- **动画库**: Animate.css 4.1.1

## 项目结构

```
.
├── app/                    # 后端应用核心代码
│   ├── __init__.py        # Application Factory
│   ├── models/            # 数据库模型
│   │   ├── laboratory.py  # 实验室模型
│   │   ├── equipment.py   # 设备模型
│   │   ├── student.py     # 学生模型
│   │   ├── teacher.py     # 教师模型
│   │   ├── admin.py       # 管理员模型
│   │   ├── reservation.py # 预约模型
│   │   ├── timeslot.py    # 时间段模型
│   │   ├── auditlog.py    # 审计日志模型
│   │   └── mixins.py      # 模型混入类
│   ├── api/               # API 蓝图
│   │   └── v1/           # API v1 版本
│   │       ├── laboratory.py  # 实验室 API
│   │       └── schemas/       # API 序列化模式
│   ├── services/          # 业务逻辑层
│   │   └── lab_service.py # 实验室服务
│   └── utils/             # 工具类
│       ├── response.py    # 统一响应格式
│       ├── exceptions.py # 异常处理
│       └── schemas.py     # 通用模式
├── frontend/              # 前端应用
│   ├── src/
│   │   ├── api/          # API 请求封装
│   │   ├── components/   # Vue 组件
│   │   ├── views/        # 页面视图
│   │   ├── router/       # 路由配置
│   │   ├── assets/       # 静态资源
│   │   └── App.vue       # 根组件
│   ├── package.json      # 前端依赖
│   └── vite.config.js    # Vite 配置
├── migrations/            # 数据库迁移文件
├── config.py              # 后端配置文件
├── run.py                 # 后端启动入口
├── requirements.txt       # 后端依赖包
├── env.example           # 环境变量示例
└── README.md             # 项目说明
```

## 功能模块

### 核心功能

- **实验室管理**: 实验室信息管理、实验室列表查询
- **设备管理**: 设备信息管理、设备状态跟踪
- **预约管理**: 设备预约申请、预约审批、预约查询
- **用户管理**: 学生、教师、管理员三种角色管理
- **时间段管理**: 设备可用时间段配置
- **审计日志**: 操作记录追踪

### 用户角色

- **学生**: 查看设备信息、提交预约申请、查看预约状态
- **教师**: 查看设备信息、提交预约申请、查看预约状态
- **管理员**: 实验室管理、设备管理、预约审批、用户管理

## 快速开始

### 环境要求

- Python 3.8+
- Node.js 16+
- MySQL 或 TiDB Cloud

### 后端启动

#### 1. 安装依赖

```bash
pip install -r requirements.txt
```

#### 2. 配置环境变量

复制 `env.example` 为 `.env` 并填写配置：

```bash
# Windows
copy env.example .env

# Linux/Mac
cp env.example .env
```

编辑 `.env` 文件，配置数据库连接信息：

```env
# Flask 环境配置
FLASK_ENV=development
FLASK_DEBUG=True
FLASK_HOST=0.0.0.0
FLASK_PORT=5000

# Flask 密钥（生产环境请务必修改）
SECRET_KEY=your-secret-key-here

# TiDB Cloud 数据库配置
DB_USER=your_username
DB_PASSWORD=your_password
DB_HOST=your_tidb_host
DB_PORT=4000
DB_NAME=instrument_booking

# SSL 配置（TiDB Cloud 需要）
SSL_VERIFY_CERT=True
SSL_VERIFY_IDENTITY=True
```

#### 3. 初始化数据库

```bash
# 初始化迁移目录（如果还没有）
flask db init

# 创建初始迁移
flask db migrate -m "Initial migration"

# 应用迁移
flask db upgrade
```

#### 4. 运行后端服务

```bash
python run.py
```

或使用 Flask CLI：

```bash
flask run
```

后端服务将在 `http://localhost:5000` 启动。

#### 5. 访问 API 文档

启动应用后，访问 Swagger UI 文档：

```
http://localhost:5000/apidocs
```

### 前端启动

#### 1. 安装依赖

```bash
cd frontend
npm install
```

#### 2. 配置 API 地址

编辑 `frontend/src/api/request.js`，配置后端 API 地址（默认已配置为 `http://localhost:5000`）。

#### 3. 启动开发服务器

```bash
npm run dev
```

前端应用将在 `http://localhost:5173` 启动（Vite 默认端口）。

#### 4. 构建生产版本

```bash
npm run build
```

构建产物将输出到 `frontend/dist` 目录。

## 环境变量说明

### 后端环境变量

| 变量名 | 说明 | 默认值 |
|--------|------|--------|
| `FLASK_ENV` | Flask 环境 (development/testing/production) | development |
| `FLASK_DEBUG` | 是否开启调试模式 | True |
| `FLASK_HOST` | 监听地址 | 0.0.0.0 |
| `FLASK_PORT` | 监听端口 | 5000 |
| `SECRET_KEY` | Flask 密钥 | dev-secret-key-change-in-production |
| `DB_USER` | 数据库用户名 | root |
| `DB_PASSWORD` | 数据库密码 | - |
| `DB_HOST` | 数据库主机 | localhost |
| `DB_PORT` | 数据库端口 | 4000 |
| `DB_NAME` | 数据库名称 | instrument_booking |
| `SQLALCHEMY_DATABASE_URI` | 数据库连接 URI（可选，覆盖单独配置） | - |
| `SQLALCHEMY_ECHO` | 是否打印 SQL 语句 | False |
| `SSL_VERIFY_CERT` | 是否验证 SSL 证书 | True |
| `SSL_VERIFY_IDENTITY` | 是否验证 SSL 身份 | True |
| `SSL_CA` | SSL CA 证书路径（可选） | - |
| `MIGRATE_DIRECTORY` | 迁移文件目录 | migrations |

## 数据库模型

### 核心模型

- **Laboratory**: 实验室信息
- **Equipment**: 设备信息
- **Student**: 学生信息
- **Teacher**: 教师信息
- **Admin**: 管理员信息
- **Reservation**: 预约记录
- **TimeSlot**: 时间段配置
- **AuditLog**: 审计日志

所有模型都继承自 `ToDictMixin`，提供统一的序列化方法。

## 开发规范

### Application Factory 模式

项目采用 Flask Application Factory 模式，所有扩展都在 `create_app()` 函数中初始化，便于测试和部署。

### 目录说明

- **models/**: 数据库模型定义，使用 SQLAlchemy ORM
- **api/v1/**: API 路由，按版本组织，使用 Flask Blueprint
- **services/**: 业务逻辑层，处理复杂业务逻辑，与数据库操作分离
- **utils/**: 工具类，包括统一响应格式、异常处理、通用模式等

### 统一响应格式

使用 `app/utils/response.py` 中的工具函数：

```python
from app.utils.response import success_response, error_response

# 成功响应
return success_response(data={...}, message='操作成功')

# 错误响应
return error_response(message='操作失败', status_code=400)
```

### 异常处理

使用 `app/utils/exceptions.py` 中定义的异常类：

```python
from app.utils.exceptions import ValidationError, NotFoundError

raise ValidationError('数据验证失败', errors={...})
raise NotFoundError('资源未找到')
```

### API 版本管理

API 按版本组织在 `app/api/v1/` 目录下，便于后续版本升级和维护。

## 数据库迁移

```bash
# 创建迁移
flask db migrate -m "描述信息"

# 应用迁移
flask db upgrade

# 回滚迁移
flask db downgrade

# 查看迁移历史
flask db history
```

## API 接口

### 基础路径

所有 API 接口的基础路径为：`/api/v1`

### 主要接口

- `GET /api/v1/laboratories` - 获取实验室列表
- `GET /api/v1/laboratories/:id` - 获取实验室详情
- 更多接口请查看 Swagger 文档：`http://localhost:5000/apidocs`

## 前端路由

- `/` - 首页
- `/equipment` - 仪器目录
- `/laboratories` - 实验室列表
- `/reservations` - 预约管理
- `/help` - 帮助中心

## 许可证

MIT License

## 贡献

欢迎提交 Issue 和 Pull Request！
