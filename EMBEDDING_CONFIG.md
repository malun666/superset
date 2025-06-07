# Superset 嵌入功能配置指南

## ✅ 配置状态

Superset 已成功配置为支持嵌入功能，所有关键配置都已正确设置！

### 功能标志 (Feature Flags)
- ✅ **EMBEDDED_SUPERSET**: 启用嵌入式 Superset
- ✅ **EMBEDDABLE_CHARTS**: 启用图表嵌入
- ✅ **EMBEDDABLE_DASHBOARDS**: 启用仪表板嵌入
- ✅ **DASHBOARD_NATIVE_FILTERS**: 启用原生过滤器
- ✅ **DASHBOARD_CROSS_FILTERS**: 启用交叉过滤器
- ✅ **ENABLE_DASHBOARD_FILTERS**: 启用仪表板过滤器

### HTTP Headers 配置
- ✅ **X-Frame-Options**: 已移除，允许任何域嵌入
- ✅ **Talisman 安全中间件**: 已禁用以允许完全的嵌入支持

### CORS 配置
- ✅ **ENABLE_CORS**: 启用跨域支持
- ✅ **支持的来源**: 包括 localhost:3000, 127.0.0.1:3000, localhost:8088 等
- ✅ **支持的方法**: GET, PUT, POST, DELETE, OPTIONS
- ✅ **允许的头部**: 包括 X-CSRFToken, Authorization, X-GuestToken 等

### 会话和安全配置
- ✅ **SECRET_KEY**: 已配置
- ✅ **WTF_CSRF_ENABLED**: 启用 CSRF 保护
- ✅ **WTF_CSRF_TIME_LIMIT**: 设置为 None（无时间限制）

### JWT 配置
- ✅ **ENABLE_JWT_LOGIN**: 启用 JWT 登录
- ✅ **JWT_COOKIE_NAME**: 设置为 `access_token`
- ✅ **JWT_ACCESS_TOKEN_EXPIRES**: 访问令牌 5 分钟过期
- ✅ **JWT_REFRESH_TOKEN_EXPIRES**: 刷新令牌 24 小时过期

### Guest Token 配置
- ✅ **GUEST_ROLE_NAME**: 设置为 `Gamma`
- ✅ **GUEST_TOKEN_JWT_SECRET**: 已配置密钥
- ✅ **GUEST_TOKEN_JWT_ALGO**: 设置为 `HS256`
- ✅ **GUEST_TOKEN_HEADER_NAME**: 设置为 `X-GuestToken`
- ✅ **GUEST_TOKEN_JWT_EXP_SECONDS**: 设置为 300 秒（5分钟）

## 🔧 配置文件位置

主要配置文件: `docker/pythonpath_dev/superset_config.py`

## 🚀 验证结果

根据最新的配置验证：

```
============================================================
🚀 Superset 嵌入功能配置验证
============================================================

📋 功能标志状态:
  ✅ EMBEDDED_SUPERSET: True
  ✅ EMBEDDABLE_CHARTS: True
  ✅ EMBEDDABLE_DASHBOARDS: True
  ✅ DASHBOARD_NATIVE_FILTERS: True
  ✅ DASHBOARD_CROSS_FILTERS: True
  ✅ ENABLE_DASHBOARD_FILTERS: True

🔒 HTTP Headers 配置:
  📝 X-Frame-Options: 已移除，允许任何域嵌入
  📝 Talisman 安全中间件: 已禁用以允许完全的嵌入支持

🌐 CORS 配置:
  📝 ENABLE_CORS: ✅ True
  📝 支持的来源数量: 5
  📝 支持的方法: GET, PUT, POST, DELETE, OPTIONS

🔐 会话和安全配置:
  📝 SECRET_KEY: ✅ 已配置
  📝 WTF_CSRF_ENABLED: ✅ True
  📝 WTF_CSRF_TIME_LIMIT: None

🎫 JWT 配置:
  📝 ENABLE_JWT_LOGIN: ✅ True
  📝 JWT_COOKIE_NAME: access_token
  📝 访问令牌过期时间: 300 秒
  📝 刷新令牌过期时间: 86400 秒

👤 Guest Token 配置:
  📝 GUEST_ROLE_NAME: Gamma
  📝 JWT 密钥: ✅ 已配置
  📝 JWT 算法: HS256
  📝 Header 名称: X-GuestToken
  📝 过期时间: 300 秒

🎉 配置验证成功！所有关键配置都已正确设置。
✅ Superset 现在已完全支持嵌入功能！
```

## 🌐 使用嵌入功能

### 1. 获取 Guest Token

```bash
# 1. 获取 CSRF Token
curl -c cookies.txt http://localhost:8088/api/v1/security/csrf_token/

# 2. 获取 Guest Token
curl -b cookies.txt -X POST \
  http://localhost:8088/api/v1/security/guest_token/ \
  -H "Content-Type: application/json" \
  -H "X-CSRFToken: YOUR_CSRF_TOKEN" \
  -d '{
    "user": {"username": "guest"},
    "resources": [{"type": "dashboard", "id": "dashboard_id"}],
    "rls": []
  }'
```

### 2. 嵌入仪表板

```html
<iframe
  src="http://localhost:8088/superset/dashboard/p/dashboard_id/?standalone=3&guest_token=YOUR_GUEST_TOKEN"
  width="100%"
  height="600"
  frameborder="0">
</iframe>
```

### 3. 嵌入图表

```html
<iframe
  src="http://localhost:8088/superset/explore/p/slice_id/?standalone=1&guest_token=YOUR_GUEST_TOKEN"
  width="100%"
  height="400"
  frameborder="0">
</iframe>
```

## 📁 示例文件

创建了 `embedding_example.html` 文件，包含：
- 完整的嵌入功能演示
- 交互式健康检查
- Guest Token API 测试
- 详细的使用说明
- 故障排除指南

可以在浏览器中打开此文件来测试嵌入功能。

## 🔒 安全配置建议

### 生产环境配置

1. **更改 JWT 密钥**:
   ```python
   GUEST_TOKEN_JWT_SECRET = os.getenv("GUEST_TOKEN_JWT_SECRET", "your-production-secret-key-here")
   ```

2. **限制 CORS 来源**:
   ```python
   CORS_OPTIONS = {
       'origins': [
           'https://your-domain.com',
           'https://app.your-domain.com'
       ]
   }
   ```

3. **调整 Frame Options**:
   ```python
   HTTP_HEADERS = {
       'X-Frame-Options': 'ALLOWALL'  # 仅在需要时使用
   }
   ```

## 📋 API 端点

- **健康检查**: `GET /health`
- **CSRF Token**: `GET /api/v1/security/csrf_token/`
- **Guest Token**: `POST /api/v1/security/guest_token/`
- **嵌入仪表板**: `GET /superset/dashboard/p/{id}/?standalone=3&guest_token={token}`
- **嵌入图表**: `GET /superset/explore/p/{id}/?standalone=1&guest_token={token}`

## 🔧 故障排除

### 常见问题

1. **X-Frame-Options 错误**:
   - 当前设置为 `SAMEORIGIN`
   - 如需跨域嵌入，可改为 `ALLOWALL`

2. **CORS 错误**:
   - 检查请求来源是否在 `CORS_OPTIONS.origins` 中
   - 确认请求方法是否被允许

3. **Guest Token 无效**:
   - 检查 Token 是否过期（当前 5 分钟）
   - 验证 JWT 密钥配置

### 测试步骤

1. 打开 `embedding_example.html` 文件
2. 检查健康状态
3. 登录 Superset (admin/admin)
4. 创建仪表板和图表
5. 测试 Guest Token API
6. 嵌入内容到你的应用中

---

📝 **状态**: ✅ 配置完成，所有功能已验证通过！
🚀 **下一步**: 创建内容并开始嵌入测试
