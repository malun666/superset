# Superset with MS SQL Server Support

这个 Superset 安装已经配置为支持 Microsoft SQL Server 数据库连接。

## 已安装的组件

- **FreeTDS**: 用于连接 SQL Server 的开源库
- **pymssql**: Python SQL Server 数据库驱动

## 部署状态

✅ Docker 镜像已构建: `superset-mssql:latest`
✅ 容器已启动并运行
✅ MS SQL Server 驱动已验证

## 访问 Superset

1. **Web 界面**: http://localhost:8088
2. **默认登录信息**:
   - 用户名: `admin`
   - 密码: `admin`

## 连接 MS SQL Server 数据库

1. 登录到 Superset Web 界面
2. 点击 **Settings** > **Database Connections**
3. 点击 **+ Database** 添加新数据库
4. 选择 **Microsoft SQL Server** 作为数据库类型
5. 使用以下连接字符串格式:

```
mssql+pymssql://username:password@server:port/database
```

示例:
```
mssql+pymssql://sa:MyPassword123@sqlserver.example.com:1433/MyDatabase
```

## 连接字符串参数说明

- `username`: SQL Server 用户名
- `password`: SQL Server 密码  
- `server`: SQL Server 服务器地址
- `port`: SQL Server 端口 (默认 1433)
- `database`: 数据库名称

## 服务管理

启动服务:
```bash
docker-compose up -d
```

停止服务:
```bash
docker-compose down
```

查看日志:
```bash
docker logs superset_app
```

## 端口映射

- **Superset Web**: 8088
- **PostgreSQL (元数据)**: 5432
- **Redis (缓存)**: 6379
- **WebSocket**: 8080
- **前端开发服务器**: 9000

## 注意事项

- 确保您的 SQL Server 实例允许远程连接
- 检查防火墙设置，确保端口 1433 (或自定义端口) 可访问
- 对于 SQL Server Authentication，请确保启用了混合身份验证模式 
