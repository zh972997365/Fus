```markdown
# Fus - 文件上传服务

一个基于 Go 和 Gin 框架开发的文件上传服务系统，支持多用户认证、文件/文件夹上传、文件过期自动清理等功能。

## ✨ 功能特性

- **多用户认证**：支持多用户账号登录，基于 Cookie 和 Basic 认证
- **文件/文件夹上传**：
  - 支持单文件和多文件上传
  - 支持整个文件夹上传，保持目录结构
  - 自动处理文件名冲突（重命名）
- **过期时间管理**：
  - 支持文件永久保存
  - 可设置自定义过期时间（分钟/小时/天/月/年）
  - 过期文件自动清理
- **文件访问**：
  - 支持文件在线预览和下载
  - 自动生成目录列表
  - 支持直接访问 HTML 文件（index.html/home.html）
- **用户界面**：
  - 现代化的 Web 界面
  - 响应式设计，支持移动端
  - 文件上传进度显示
  - 用户使用指南弹窗

## 🛠️ 技术栈

- **后端**：Go, Gin, Gorilla/mux
- **前端**：HTML5, CSS3, JavaScript
- **存储**：本地文件系统
- **认证**：Session Cookie + Basic Auth

## 📋 系统要求

- Go 1.16 或更高版本
- 现代浏览器（Chrome, Firefox, Safari, Edge）

## 🚀 快速开始

### 1. 克隆项目

```bash
git clone https://github.com/yourusername/Fus.git
cd Fus
```

### 2. 配置文件

在项目根目录创建 `.env` 文件：

```env
# 服务器端口 (可选，默认8080)
PORT=8080

# 用户凭据 (格式: 用户名:密码, 用户名:密码)
AUTH_USERS=admin:admin123, user1:password1

# 上传路径 (可选，默认 ./storage/data)
UPLOAD_PATH=./storage/data
```

### 3. 运行服务

```bash
go run main.go
```

服务启动后，访问 `http://localhost:8080` 即可进入登录页面。

## 📁 项目结构

```
Fus/
├── main.go                 # 主程序入口
├── go.mod                  # Go模块文件
├── .env                    # 环境配置文件
├── backend/
│   ├── config/            # 配置管理
│   │   └── config.go
│   ├── handlers/          # 请求处理
│   │   ├── auth.go        # 登录/登出处理
│   │   ├── upload.go      # 文件上传处理
│   │   ├── file.go        # 文件访问处理
│   │   └── directory.go   # 目录列表生成
│   ├── middleware/        # 中间件
│   │   └── auth.go        # 认证中间件
│   ├── utils/             # 工具函数
│   │   └── utils.go       # 通用工具
│   └── cleanup/           # 清理任务
│       └── cleanup.go     # 过期文件清理
├── frontend/
│   ├── static/            # 静态资源
│   │   ├── style.css      # 样式文件
│   │   └── ico.svg        # 网站图标
│   └── templates/         # HTML模板
│       ├── index.html     # 上传页面
│       └── login.html     # 登录页面
└── storage/               # 文件存储目录
    └── data/              # 用户文件存储
```

## 🔧 API 接口

### 认证相关

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/login` | 登录页面 |
| POST | `/login` | 登录认证 |
| GET | `/logout` | 登出 |

### 文件操作

| 方法 | 路径 | 描述 |
|------|------|------|
| POST | `/upload` | 上传文件/文件夹（需认证）|
| GET | `/:username/*filepath` | 访问文件/目录 |

### 页面路由

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/` | 上传页面（需认证）|

## 💡 使用说明

### 登录系统

1. 使用配置的账号密码登录
2. 登录成功后自动跳转到上传页面

### 上传文件/文件夹

1. **选择上传类型**：
   - 点击左侧"选择文件"上传单个或多个文件
   - 点击右侧"选择文件夹"上传整个文件夹（保持目录结构）
   
2. **设置过期时间**：
   - 永久：文件永久保存
   - 自定义：设置具体的时间和单位
   
3. **开始上传**：
   - 点击"确认上传"按钮
   - 查看上传进度
   
4. **获取分享链接**：
   - 上传成功后自动生成访问链接
   - 可复制链接分享给他人

### 文件访问

- **文件**：直接访问链接可预览或下载
- **文件夹**：显示目录列表，支持浏览
- **HTML文件**：如果文件夹包含 `index.html` 或 `home.html`，会自动显示该页面

## 🧹 文件清理机制

- 系统每 5 分钟自动扫描一次存储目录
- 检查文件过期时间，删除过期文件及其元数据
- 过期文件删除后无法恢复

## 🔒 安全特性

- 路径遍历攻击防护
- 密码安全比较（使用 `subtle.ConstantTimeCompare`）
- 用户名过滤（防止路径注入）
- 认证中间件保护敏感路由
- `.meta` 元数据文件无法直接访问

## 📝 配置说明

### 环境变量

| 变量 | 描述 | 默认值 | 示例 |
|------|------|--------|------|
| `PORT` | 服务端口 | 8080 | 8080 |
| `AUTH_USERS` | 用户凭据列表 | - | `admin:123,user:456` |
| `UPLOAD_PATH` | 文件存储路径 | `./storage/data` | `/data/uploads` |

### 用户凭据格式

```
用户名:密码,用户名:密码
```

示例：
```
admin:admin123, john:pass123, jane:secret
```

## 🐛 常见问题

### 1. 上传失败怎么办？
- 检查文件大小是否超过 1GB
- 确认服务器存储空间充足
- 查看网络连接状态

### 2. 文件访问404？
- 确认文件未过期
- 检查URL路径是否正确
- 确认用户目录存在

### 3. 登录失败？
- 检查 `.env` 文件中的用户凭据配置
- 确认密码输入正确
- 查看浏览器 Cookie 设置

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request！

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启 Pull Request

## 📄 开源协议

本项目采用 MIT 协议，详见 [LICENSE](LICENSE) 文件。

## 📧 联系方式

如有问题或建议，请通过 Issue 联系我们。

---

**注意**：生产环境使用时，请确保：
- 使用 HTTPS 协议
- 修改默认密码
- 定期备份重要文件
- 监控磁盘空间使用情况
```