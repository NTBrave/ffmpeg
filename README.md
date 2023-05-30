### 说明
使用切片工具ffmpge对视频进行切片，ffmpeg mp4 AES-128加密 ts 分片处理，
使用openssl生成密钥，在切片过程中加密
### 工具
ffmpeg
openssl

### 作用
视频被下载之后是不能直接播放的

### 参看
https://blog.csdn.net/shgh_2004/article/details/107249816

1.加密用的key（文件则保存当前目录）
```bash
指令：openssl rand -base64 20 > enc.key 
#提示打开文件本次生成的 n4DHLx7kMPeewvW3dGlm5i/EE8I=
```

2.另一个是iv（生成一段字符串，记下来）：
```bash
指令：openssl rand -hex 16 
#提示打印出本次生成的682f5033538cf71567e1bdb38f5f9a07
```

新建一个文件enc.keyinfo 内容格式如下：
```bash
Key URI # enc.key的路径，使用http形式
Path to key file   # enc.key文件
IV # 上面生成的iv
```

实例：
```bash
http://edu.gamagou.cn/enc.key
/usr/share/nginx/html/enc.key
682f5033538cf71567e1bdb38f5f9a07
```

执行命令：
```bash
G:\code\ffmpeg\ffmpeg-master-latest-win64-gpl\bin\ffmpeg.exe -i G:\code\ffmpeg\html\video.mp4 -hls_time 5 -hls_key_info_file G:\code\ffmpeg\html\enc\enc.keyinfo -hls_playlist_type vod  -y G:\code\ffmpeg\html\out\index.m3u8
```