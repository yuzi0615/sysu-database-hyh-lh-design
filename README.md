# sysu-database-hyh-lh-design
中山大学23级本科生数据库系统实验大作业
前后端及数据库文件在master分支

# 食堂外卖配送管理系统

## 项目简介

这是一个基于Vue.js和Flask开发的食堂外卖配送管理系统，实现了用户下单、订单管理、配送管理等功能。系统分为前端用户界面和后端管理界面，支持用户注册登录、浏览店铺、下单支付、查看订单状态等操作，同时为管理员提供店铺管理、服务人员管理、配送员管理、物流管理等功能。

## 技术栈

### 前端
- Vue.js 2.6.14
- Vue Router 3.5.2
- Element UI 2.15.12
- Axios 1.2.0

### 后端
- Flask 2.3.3
- Flask-SQLAlchemy 3.1.1
- Flask-CORS 4.0.0
- Redis 5.0.1
- PyJWT 2.8.0
- MySQL/PostgreSQL (根据配置)

## 功能特性

### 用户功能
- 用户登录
- 浏览店铺信息
- 下单支付
- 查看订单状态（未发送、配送中、已完成）
- 修改个人信息
- 修改密码

### 管理员功能
- 店铺管理（添加、修改、删除店铺）
- 服务人员管理（添加、删除服务人员）
- 配送员管理（添加、删除配送员）
- 订单管理（处理未发送订单、查看配送中订单、查看已完成订单）
- 物流管理（查看未完成物流、查看已完成物流）

## 项目结构

```
├── 前端代码/
│   └── sjk/
│       ├── public/
│       │   └── index.html
│       ├── src/
│       │   ├── api/
│       │   │   └── index.js         # API请求配置
│       │   ├── components/
│       │   │   ├── ManageOrder/     # 订单管理组件
│       │   │   ├── ManageWuliu/     # 物流管理组件
│       │   │   ├── UserMsg/         # 用户信息组件
│       │   │   ├── UserOrder/       # 用户订单组件
│       │   │   ├── ManageDispatcher.vue  # 配送员管理
│       │   │   ├── ManageServer.vue      # 服务人员管理
│       │   │   ├── ManageShop.vue        # 店铺管理
│       │   │   ├── MyLogReg.vue          # 登录注册
│       │   │   ├── MyManage.vue          # 管理界面
│       │   │   ├── MyUser.vue            # 用户界面
│       │   │   └── UserShop.vue          # 店铺浏览
│       │   ├── router/
│       │   │   └── index.js         # 路由配置
│       │   ├── App.vue              # 根组件
│       │   └── main.js              # 入口文件
│       ├── .gitignore
│       ├── babel.config.js
│       ├── jsconfig.json
│       ├── package-lock.json
│       ├── package.json
│       └── vue.config.js
├── 后端代码/
│   ├── venv/                        # 虚拟环境
│   ├── app.py                       # 主应用
│   ├── auth.py                      # 认证相关
│   ├── config.py                    # 配置文件
│   ├── main.py                      # 入口文件
│   └── requirements.txt             # 依赖包
```

## 安装部署

### 前端部署

1. 进入前端目录
   ```bash
   cd 前端代码/sjk
   ```

2. 安装依赖
   ```bash
   npm install
   ```

3. 启动开发服务器
   ```bash
   npm run serve
   ```


### 后端部署

1. 进入后端目录
   ```bash
   cd 后端代码
   ```

2. 安装依赖
   ```bash
   pip install -r requirements.txt
   ```

3. 配置数据库连接（修改config.py）

4. 启动后端服务器
   ```bash
   python app.py
   ```

## API 接口

### 用户相关
- `POST /api/user/login` - 用户登录
- `GET /api/user/shop` - 获取店铺信息
- `POST /api/user/addorder` - 下订单
- `GET /api/user/unsend` - 获取未发送订单
- `GET /api/user/sending` - 获取配送中订单
- `GET /api/user/sended` - 获取已完成订单
- `GET /api/user/usermsg` - 获取用户信息
- `POST /api/user/pwd_chg` - 修改密码

### 管理员相关
- `GET /api/manager/shop` - 获取店铺信息
- `POST /api/manager/shop` - 添加/修改店铺
- `DELETE /api/manager/shop` - 删除店铺
- `GET /api/manager/server` - 获取服务人员信息
- `POST /api/manager/server` - 添加服务人员
- `DELETE /api/manager/server` - 删除服务人员
- `GET /api/manager/dispatcher` - 获取配送员信息
- `POST /api/manager/dispatcher` - 添加配送员
- `DELETE /api/manager/dispatcher` - 删除配送员
- `GET /api/manager/wuliu` - 获取物流信息
- `GET /api/manager/unsend` - 获取未发送订单
- `POST /api/manager/unsend` - 派发订单
- `GET /api/manager/sending` - 获取配送中订单
- `GET /api/manager/sended` - 获取已完成订单

## 数据库设计

系统使用以下主要数据表：
- `user` - 用户信息表
- `fastfood_shop` - 店铺信息表
- `server` - 服务人员表
- `dispatcher` - 配送员表
- `oorder` - 订单表
- `sending_order` - 配送中订单表
- `sended_order` - 已完成订单表
- `wuliu` - 物流信息表
- `user_msg` - 用户详细信息表

## 配置说明

### 前端配置
- API 基础地址：修改 `src/api/index.js` 中的 `baseURL`

### 后端配置
- 数据库连接：修改 `config.py` 中的数据库配置
- Redis 配置：修改 `config.py` 中的 Redis 连接信息
- JWT 密钥：修改 `auth.py` 中的密钥配置


## 注意事项

1. 系统使用 JWT 进行身份验证，请确保在生产环境中使用安全的密钥
2. 短信验证码功能已注释，如需使用请配置阿里云短信服务
3. 请确保数据库已创建并导入相关表结构
4. 开发环境中使用的是本地数据库和 Redis，请根据实际情况修改配置

## 贡献

欢迎提交 Issue 和 Pull Request 来改进这个项目。

## 许可证

MIT
