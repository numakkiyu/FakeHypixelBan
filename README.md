# Fake Hypixel Banned

这是就是假的Hypixel Ban 用于整蛊他人，完全采用Go语言开发，特别精简，可以自定义封禁原因信息，原本这个是整合到新的项目，只是觉得有趣单独的拉了出来建立新库，单纯是无聊，可以显示服务器列表信息并返回自定义的封禁消息。

## 特别鸣谢

- 项目灵感提供 [IAFEnvoy](https://github.com/IAFEnvoy)  提供了开源代码
- 感谢另外一个 [Juzi_CN](https://github.com/juzicn) 不知道感谢什么，只是单纯的想把他拉出来

## 在线体验

在多人游戏添加下面这个地址

```bash
hyp.jsip.hypcvgm.top
```

## 功能特点

1. 模拟 Hypixel 服务器的 MOTD 显示
   - 显示服务器名称和版本范围
   - 显示活动信息
   - 显示在线人数
   - 显示服务器图标

2. 自定义封禁消息
   - 显示剩余封禁时间
   - 显示封禁原因
   - 显示申诉链接
   - 显示封禁 ID

## 使用方法

1. 编译运行

```bash

#直接运行
go run mc_main.go

# 编译
go build mc_main.go

# 运行
./mc_main
```

2. 配置
- 默认监听端口：25565（标准 Minecraft 服务器端口）
- 支持的 Minecraft 版本：1.8-1.21
- 在线人数显示：可以自定义

3. 自定义修改

```go
// 发送Fake Hypixel Banned消息
message := DisconnectMessage{
				Text: strings.Join([]string{
					"§cYou are temporarily banned for §f29d 24h 59m 59s §cfrom this server!\n\n",
					"§7Reason: §fCheating through the use of unfair game advantages.\n",
					"§7Find out more: §b§nhttps://www.hypixel.net/appeal§r\n\n",
					"§7Ban ID: §f#9BE61827\n",
					"§7Sharing your Ban ID may affect the processing of your appeal!",
				}, ""),
			}

...
// MOTD信息
	status := StatusResponse{
		Version: Version{
			Name:     "1.8-1.21",
			Protocol: 47,
		},
		Players: Players{
			Max:    200000,
			Online: 25909,
		},
		Description: Description{
			Text: "                §aHypixel Network §c[1.8-1.21]\n" +
				"§c§lHOLIDAY EVENT §r| §6§lDISASTERS §r| §d§lMOUNTAINTOP",
		},
		Favicon: serverIcon,
	}
```

- 修改 MOTD：更改 `handleStatusRequest` 函数中的 `Description.Text` 字段
- 修改版本范围：更改 `Version.Name` 字段
- 修改在线人数：更改 `Players.Online` 和 `Players.Max` 字段
- 修改封禁消息：更改 `handleConnection` 函数中的 `message` 变量

## 颜色代码说明

- §a - 绿色
- §b - 淡蓝色
- §c - 红色
- §d - 粉色
- §e - 黄色
- §f - 白色
- §6 - 金色
- §7 - 灰色
- §l - 粗体
- §n - 下划线
- §r - 重置格式

## 注意事项

1. 确保服务器端口（25565）未被占用
2. 如果部署在云服务器上，需要开放对应端口的防火墙规则
3. 程序会记录所有连接和错误信息到控制台

## 常见问题

1. 无法启动服务器
   - 检查端口是否被占用
   - 确认是否有管理员权限

2. 客户端无法连接
   - 检查防火墙设置
   - 确认服务器 IP 和端口配置正确

3. MOTD 显示异常
   - 检查颜色代码格式
   - 确认文本编码为 UTF-8

## 示例输出

服务器启动时：

```
Fake Hypixel 服务器已启动在端口25565...
```

收到连接时：
```
收到连接: 版本=47, 地址=xxx.xxx.xxx.xxx, 端口=25565, 状态=1
状态响应已发送
收到ping请求: xxxxxxxxx
pong响应已发送
```

