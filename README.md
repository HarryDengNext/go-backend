# GoBackend 项目文档

## 项目概述
GoBackend 是一个基于 Go 语言开发的博客后端服务，使用了 Fiber 框架作为 Web 框架，并通过 GORM 实现 ORM 数据库操作。该项目支持用户注册、登录、发布博客文章、上传图片等功能。

---

## 项目结构
以下是项目的目录结构及每个部分的作用：

```plaintext
blogbackend/
├── controller/        # 控制器层，处理业务逻辑和请求响应
├── database/          # 数据库连接与迁移相关代码
├── middleware/        # 中间件，用于身份验证等通用功能
├── models/           # 数据模型定义
├── routes/           # 路由配置
├── util/             # 工具类，如 JWT 生成与解析
└── main.go           # 项目入口文件
```

---

## 各模块详细说明

### 1. **main.go**
- **作用**：项目入口文件，初始化数据库连接、加载环境变量、启动 Fiber 应用。
- **关键功能**：
  - 加载 `.env` 文件中的环境变量。
  - 初始化数据库连接（调用 `database.Connect()`）。
  - 配置路由并启动服务器。

### 2. **controller 包**
- **作用**：实现具体的业务逻辑，处理 HTTP 请求并返回响应。
- **主要控制器**：
  - **authContoller.go**：
    - 用户注册 (`Register`) 和登录 (`Login`)。
    - 使用 JWT 进行身份认证。
  - **postController.go**：
    - 创建博客文章 (`CreatePost`)。
    - 获取所有文章 (`AllPost`)。
    - 获取单篇文章详情 (`DetailPost`)。
    - 更新文章 (`UpdatePost`)。
    - 删除文章 (`DeletePost`)。
    - 获取当前用户的博客文章 (`UniquePost`)。
  - **imageController.go**：
    - 图片上传功能 (`Upload`)，支持随机命名避免冲突。

### 3. **database 包**
- **作用**：管理数据库连接和自动迁移。
- **核心文件**：
  - **connect.go**：
    - 加载 `.env` 文件中的数据库连接字符串。
    - 使用 GORM 连接 MySQL 数据库。
    - 自动迁移数据表结构（`User` 和 `Blog` 模型）。

### 4. **middleware 包**
- **作用**：提供中间件功能，拦截请求进行权限校验。
- **核心文件**：
  - **middleware.go**：
    - `IsAuthenticate`：检查用户是否已登录，通过解析 JWT Cookie 实现。

### 5. **models 包**
- **作用**：定义数据模型及其方法。
- **核心文件**：
  - **user.go**：
    - 定义 `User` 模型，包含字段如 `Id`, `FirstName`, `LastName`, `Email`, `Password`, `Phone`。
    - 提供密码加密方法 `SetPassword` 和密码校验方法 `ComparePassword`。
  - **blog.go**：
    - 定义 `Blog` 模型，包含字段如 `Id`, `Title`, `Desc`, `Image`, `UserID`。
    - 通过外键关联 `User` 模型。

### 6. **routes 包**
- **作用**：配置 API 路由。
- **核心文件**：
  - **route.go**：
    - 注册用户相关路由（`/api/register`, `/api/login`）。
    - 注册博客相关路由（`/api/post`, `/api/allpost`, 等）。
    - 注册图片上传路由（`/api/upload-image`）。
    - 静态资源路由（`/api/uploads`）。

### 7. **util 包**
- **作用**：提供工具函数，如 JWT 的生成与解析。
- **核心文件**：
  - **helper.go**：
    - `GenerateJwt`：生成 JWT Token。
    - `Parsejwt`：解析 JWT Token 并返回用户 ID。

---

## 项目特点

1. **JWT 身份认证**：
   - 使用 JWT 实现用户身份认证，确保 API 的安全性。
   - 登录后生成 Token，存储在 Cookie 中，后续请求携带该 Token。

2. **RESTful API 设计**：
   - 遵循 RESTful 规范设计接口，清晰易用。
   - 支持常见的 CRUD 操作。

3. **Fiber 框架**：
   - 使用高性能的 Fiber 框架，简化路由配置和请求处理。

4. **GORM ORM 框架**：
   - 使用 GORM 实现数据库操作，减少手动 SQL 编写。
   - 支持自动迁移，简化数据库表结构管理。

5. **MySQL 数据库**：
   - 使用 MySQL 存储用户和博客数据，支持高并发和复杂查询。

6. **图片上传功能**：
   - 支持图片上传，并生成随机文件名避免冲突。
   - 提供静态资源访问路径。

---

## 使用到的技术

1. **编程语言**：
   - Go 语言

2. **Web 框架**：
   - Fiber

3. **ORM 框架**：
   - GORM

4. **数据库**：
   - MySQL

5. **身份认证**：
   - JWT (JSON Web Token)

6. **环境变量管理**：
   - godotenv

7. **密码加密**：
   - bcrypt

8. **图片上传**：
   - Fiber 的 MultipartForm 功能

9. **随机字符串生成**：
   - 使用 `math/rand` 生成随机字符串

---

## 总结
GoBackend 是一个功能完善的博客后端服务，采用了现代化的 Go 开发技术栈，具备良好的扩展性和性能表现。通过合理的模块划分和清晰的代码结构，为开发者提供了易于维护和扩展的基础框架。
