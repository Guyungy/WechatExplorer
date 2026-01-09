### 准备
Mac 电脑，arm 和 intel 都可以，关闭 SIP，如何关闭 SIP 参考下一步。
微信版本不要超过 3.7.3，非 APPstore 下载版本（听说的，没验证过）。这里有个 3.2.2 版本，今天 2024 年 8 月 2 日，测试正常。

### 关闭 SIP
Intel MacBook 按下开机键后按住 Command-R 组合键进入恢复模式，arm MacBook 按住开机键不放直到出现选择界面，进入恢复模式进行操作。

直到出现恢复模式界面。点击顶部 Utilities 菜单，然后打开终端。最后输入

```~~csrutil disable~~```

重启电脑

获取密码内存信息

```
(lldb) br set -n sqlite3_key  
Breakpoint 1: 2 locations.  
(lldb) c  
Process 58802 resuming
(lldb) memory read --size 1 --format x --count 32 $rsi
0x60000181d720: 0xe5 0x16 0xc0 0x2a 0x53 0xe3 0x44 0x58
0x60000181d728: 0x97 0x4a 0xbf 0x59 0x22 0xf1 0x8a 0x59
0x60000181d730: 0x4b 0x2a 0xe2 0x3e 0x44 0x8b 0x4e 0x55
0x60000181d738: 0x9c 0x40 0xd8 0xb6 0x74 0xf9 0xdc 0xd4
(lldb) exit
Quitting LLDB will detach from one or more processes. Do you really want to proceed: [Y/n] y
``` 


密码输出
```
source = """
0x000000000000: 0xab 0xcd 0xef 0xab 0xcd 0xef 0xab 0xcd
0x000000000008: 0xab 0xcd 0xef 0xab 0xcd 0xef 0xab 0xcd
0x000000000010: 0xab 0xcd 0xef 0xab 0xcd 0xef 0xab 0xcd
0x000000000018: 0xab 0xcd 0xef 0xab 0xcd 0xef 0xab 0xcd
"""
key = '0x' + ''.join(i.partition(':')[2].replace('0x', '').replace(' ', '') for i in source.split('\n')[1:5])

print(key)
```

导出聊天记录文件
# 在桌面上创建 WeChatDB 目录  
mkdir ~/Desktop/WeChatDB  
  
# 切换到数据库文件所在的目录，不要直接复制此处的路径，直接从 Finder 拉到终端里  
cd ~/Library/Containers/com.tencent.xinWeChat/Data/Library/Application\ Support/com.tencent.xinWeChat/2.0b4.0.9/5a22781f14219edfffa333cb38aa92cf/Message  
  
# 把所有的 msg 数据库文件拷贝到 WeChatDB 目录  
cp msg*.db ~/Desktop/WeChatDB  
  