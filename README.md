<p align="center">
  <a href="https://www.treadstone.design/" target="blank"><img src="https://www.treadstone.design/logo.png" width="120" alt="Treadstone Logo" /></a>
</p>

<h3 align="center">轻量低代码应用开发框架</h3>
<p align="center">
  <a href="https://www.treadstone.design/" target="blank" >官方网站</a>
  <a href="https://docs.treadstone.design/" target="blank" >官方文档</a>
</p>

# 介绍

### Treadstone 是什么

Treadstone 是一款个性化的低代码应用开发框架，基于 Nodejs + React.js 开发，通过可视化配置的方式，结合数据库连接、API 接口、物料库等功能辅助开发者快速搭建应用并生成代码。

### 技术选项

- NestJS
- Prisma
- React.js
- Ant Design Pro

### 功能特性

- **应用搭建及代码生成。** 后端目前支持生成 NodeJS(NestJS + Prisma + MySQL)相关模板代码，前端目前支持生成 ReactJS(Ant Design Pro)代码；
- **数据库连接。** 支持加入数据库连接，用于在应用搭建时快速导入现有数据库字段；
- **API 接口导入。** 支持将 Swagger 接口文档 JSON 数据导入到系统内，用于快速创建前端组件所需字段；
- **物料库管理。** 支持添加或导入 Treadstone 封装好的物料库，支持根据开发者习惯调整组件属性，自行添加的物料组件可以是自行封装的 React 组件也可以是任意 React UI 库组件；
- **组织管理。** 支持在系统内添加组织、部门、成员，也可以同步钉钉、企业微信组织架构；
- **统一登录。** 系统支持单点登录，支持账号密码、钉钉扫码、企业微信扫码等登录方式；
- **权限管理。** 支持添加权限资源并绑定角色，权限资源也支持复用应用数据快速创建；
- **命令行工具。** 结合 TreadstoneCLI 使用，搭建完应用之后可以快速在本地初始化及同步应用项目代码；

### Treadstone 框架的优势

- **全栈 Typescript。** Typescript 类型的优点体现贯穿前后端技术栈，从 Prisma 到 NenstJS 到 API 接口文档再到 React 组件。类型安全大量减少代码编写低级错误，自动生成的类型定义文件及类型推导也提高了开发者编码效率。得益于代码自动生成功能，开发者无需繁琐地手动编写常见接口的类型文件；
- **极尽的复用思维。** 数据库表字段导入、接口对象字段导入、基于后端自动生成前端通用列表等功能尽可能减少开发者手动创建的工作量；
- **内置企业应用基础能力。** 单点登录、统一鉴权、组织管理、权限管理，部分功能已集成钉钉、企业微信相关能力，后续还会开发/集成更多功能，让开发者无需重复开发集成，开箱配置即可使用。如果不需要这些功能也可以关闭或删除相关代码，没有强制耦合绑定；
- **可私有化部署。** 开发者完全可以把 Treadstone 私有化部署到私人服务器上，相关数据存储到私人数据库中独立维护，避免数据托管到平台可能会出现的安全问题。

# 快速安装

### 前置依赖

- Nodejs 20^
- MySQL 8.0^
- Redis

### 安装

#### 1.全局安装TreadstoneCLI 命令行工具：

```bash
npm install -g treadstone-cli
```

#### 2. 将Treadstone安装到当前文件夹：

```bash
stone install --verison standard
```

#### 3. 安装依赖：

```bash
cd treadstone-standard && npm install
```

#### 4. 修改全局配置文件：

- 将 DATABASE_URL、REDIS_URL 分别改为你的 MySQL、Redis 数据库链接地址；   
- JWT_SECRET 是 JWT 密钥，安装时会随机初始化一个，建议自行更换。如果使用Treadstone提供的单点登录、统一鉴权，请保证其他需要鉴权的后端应用 JWT 密钥与Treadstone的 JWT 密钥一致。  
- 组织密钥获取方式参考<a target="_blank" href="/docs/org-secret">获取组织密钥</a>。  

```bash title=".env"
# MySQL
DATABASE_URL="mysql://USERNAME:PASSWORD@HOST:PORT/DATABASE"
# Redis
REDIS_URL="redis://USERNAME:PASSWORD@HOST:PORT"

PORT=9000

# JWT配置
JWT_EXPIRED="7 days"
JWT_SECRET="<YOUR_JWT_SECRET>"

# 组织ID及组织密钥可在Treadstone官网生成
TREADSTONE_ORG_ID="YOUR_TREADSTONE_ORG_ID"
TREADSTONE_ORG_SECRET="YOUR_TREADSTONE_ORG_SECRET"

# 如需接入钉钉同步组织架构/钉钉扫码登录，请添加以下配置:
# ASSOCIATION_PLATFORM="DINGTALK"
# DINGTALK_APP_KEY="YOUR_DINGTALK_APP_KEY"
# DINGTALK_APP_SECRET="YOUR_DINGTALK_APP_SECRET"

# 如需接入企业微信同步组织架构/企业微信扫码登录，请添加以下配置:
# ASSOCIATION_PLATFORM="WEWORK"
# WEWORK_CORP_ID="YOUR_WEWORK_CORP_ID"
# WEWORK_CORP_SECRET="YOUR_WEWORK_CORP_SECRET"

```

#### 5. 修改完全局环境变量之后，执行数据库初始化：

```bash
npx prisma migrate deploy
```

#### 6. 全局安装 PM2：

```bash
npm i -g pm2
```

#### 7. 运行 Treadstone：

```bash
pm2 start ecosystem.config.js --env=production
```

#### 8. 运行后，修改 treadstone-cli 服务指向：

```bash
# <YOUR_TREADSTONE_URL>改为你的私有化Treadstone服务运行地址。如：http://localhost:9000
stone config set --registry <YOUR_TREADSTONE_URL>
```

#### 9. 初始化组织名称及管理员账号：

```bash
stone createsuperuser
```

#### 10. 浏览器打开私有化部署后的 treadstone，使用刚才初始化的管理员账号登录即可。默认：<a target="__blank" href="http://localhost:9000">http://localhost:9000</a>


更多详情请查看<a href="https://docs.treadstone.design/" target="blank" align="center">官方文档</a>
