# CPALauncher MVP 设计草案

## 核心目标

让不会记命令、不会分辨工作目录的用户，也能稳定地在 Windows 上运行 `CLIProxyAPI`。

## 边界

### 做什么

- 管理本地 CPA 进程
- 保存 `exe` 与 `config` 路径
- 打开管理页与相关目录
- 提供托盘化运行体验

### 不做什么

- 替代现有管理页
- 接管 OAuth 逻辑本身
- 自动改写用户的 `config.yaml`

## 首次启动流程

1. 欢迎页
2. 选择 `cli-proxy-api.exe`
3. 自动尝试发现同目录 `config.yaml`
4. 未发现时手动选择 `config.yaml`
5. 校验路径
6. 保存本地设置
7. 询问是否立即启动

## 主窗口要素

- 当前状态
- exe 路径
- config 路径
- 管理页地址
- 启动按钮
- 停止按钮
- 打开管理页
- 打开配置目录
- 打开日志目录
- 重新选择路径

## 当前实现落地情况

### 已完成

- 主窗口手写布局版已经可编译运行
- 启动器设置持久化
- `exe` / `config` 选择与自动识别
- CPA 进程启动、停止、退出码感知
- 管理页可达性探测
- 托盘菜单与最小化恢复
- 推导管理页 / 日志目录 / 鉴权目录
- 最近输出诊断区

### 当前已知取舍

- 还没有做单独的“首次配置向导页”，目前是首次弹窗 + 文件选择框
- 还没有做端口冲突的深度诊断，目前以“探测已响应”提示为主
- 还没有做开机自启、自动下载 Release、多 Profile

## 状态机

- `Unconfigured`
- `Stopped`
- `Starting`
- `Running`
- `StartFailed`
- `Stopping`

## 托盘菜单

- 显示主窗口
- 启动服务
- 停止服务
- 打开管理页
- 退出启动器

## 启动器自身配置建议

建议独立保存到：

```text
%AppData%\CPALauncher\settings.json
```

建议字段：

- `ExecutablePath`
- `ConfigPath`
- `LastKnownBaseUrl`
- `LaunchOnStartup`
- `MinimizeToTrayOnClose`
- `AutoStartService`

## 实现优先级

### P1

- 配置加载 / 保存
- 启动与停止进程
- 主窗口状态展示

### P2

- 托盘
- 打开管理页 / 目录
- 启动失败提示

### P3

- 开机自启
- 多 Profile
- 自动检查端口占用
