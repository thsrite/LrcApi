# LrcApi
A Flask API For StreamMusic

## 功能

支持酷狗API获取LRC歌词、支持缓存、支持指定端口

已经打包了Linux_x86的程序，直接启动即可

其它环境也可以安装依赖之后启动app.py

默认监听28883端口，API地址`0.0.0.0:28883/lyrics`

### 启动参数

|   参数   |   类型  | 默认值 |
| -------- | -------- | -------- |
| `--port`   | int   | 28883   |
| `--auth`  | str   |     |

`--auth`参数用于header鉴权，留空则跳过鉴权。验证header中的`Authorization`或`Authentication`字段。如果鉴权不符合，则返回403响应。

也可以使用环境变量`API_AUTH`定义，其优先性低于`--auth`参数，但是更容易在Docker中部署。

## 食用方法：

### 二进制文件：

上传至运行目录，`./lrcapi --port 8080 --auth DbG91ZEZbBgNVBAs`
	
### Python源文件：
		
拉取本项目；或者下载后上传至运行目录，解压tar.gz
		
安装依赖：`pip install -r requirements.txt`
		
启动服务：`python3 app.py --port 8080 --auth DbG91ZEZbBgNVBAs`

### Linux_x86一键部署运行：

```bash
wget https://mirror.eh.cx/APP/lrcapi.sh -O lrcapi.sh && chmod +x lrcapi.sh && sudo bash lrcapi.sh
```

### Docker部署方式

```bash
docker run -d -p 28883:28883 -v /home/user/music:/music hisatri/lyricapi:latest -e API_AUTH=DbG91ZEZbBgNVBAs
```

或者，请指定一个Tag（推荐）

```bash
docker run -d -p 28883:28883 -v /home/user/music:/music hisatri/lyricapi:alpine-py1.1 -e API_AUTH=DbG91ZEZbBgNVBAs
```

非常**不建议**使用Docker部署Navidrome以及LRCAPI，但是如果你非要这么做，我也提供了以下的教程：

如果你正在使用Navidrome Docker，请将 `/home/user/music:/music` 中的 `/home/user/music` 修改为你在Navidrome中映射的主机路径。

如果你正在使用Navidrome（真的有人会本地部署Navidrome了，然后用Docker部署这东西？），请将你的音乐文件目录映射到Docker内目录；例如如果你音乐存储的目录是`/www/path/music`，请将启动命令中的映射修改为 `/www/path/music:/www/path/music`

然后访问`http://0.0.0.0:28883/lyrics`，或者使用Nginx或Apache进行反向代理及部署SSL证书。
