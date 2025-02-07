



<h1 align="center">
  青龙安装Bot监控
  <br>
</h1>

# 注意：不要运行ql bot，和原版机器人不兼容！！！

## 1.进入容器内

``` bash
# 进入青龙容器内，“qinglong” 为容器名字。
docker exec -it qinglong /bin/bash
```

## 2.安装依赖

- [x] 一键安装

```
wget https://raw.githubusercontent.com/kissyouhunter/jbot/main/install.sh && bash install.sh
```


## 3.配置tg机器人参数

```bash
# 操作环境，容器内:
# 机器人登录相关参数，存放路径：
## 填写参考：https://raw.githubusercontent.com/kissyouhunter/jbot/main/conf/bot.json
打开网页进入青龙的页面，进入配置文件，右上角选择bot.json（如无此文件，刷新网页）
或者
映射的文件夹 config/bot.json

# 监控频道相关参数，存放路径：
## 填写参考：https://raw.githubusercontent.com/kissyouhunter/jbot/main/conf/diybotset.json
打开网页进入青龙的页面，进入配置文件，右上角选择diybotset.json（如无此文件，刷新网页）
或者
映射的文件夹 config/diybotset.json

```



## 4.启动机器人

```bash
# 操作环境，容器内:
## 删除历史登录session

## 2.12.0之后的版本
rm -f /ql/data//config/user.session

## 2.12.0之前的版本
rm -f /ql/config/user.session

# 首次启动，按照提示登录tg，填手机号格式0086xxxxxxxx

## 2.12.0之后的版本
cd /ql/data && python3 -m jbot

## 2.12.0之前的版本
cd /ql && python3 -m jbot

```

![图1：首次登录授权个人tg](https://raw.githubusercontent.com/curtinlv/gd/main/img/p1.png)

### ∆出现以上提示，即登录成功，按`ctrl+c `终止，继续以下操作：

```bash
# 通过pm2 后台启动，除了登录验证外，建议使用pm2启动机器人

# 2.12.0之后的版本
cd /ql/data/jbot/ && pm2 start ecosystem.config.js #第一次启动是这样启动，后续启动参考底部相关命令

# 2.12.0之前的版本
cd /ql/jbot/ && pm2 start ecosystem.config.js #第一次启动是这样启动，后续启动参考底部相关命令

# 查看日志：看看有没有报错。

# 2.12.0之后的版本
tail -100f /ql/data/log/bot/run.log

# 2.12.0之前的版本
tail -100f /ql/log/bot/run.log

# 终止查看日志 按 Ctrl+C

# 配置文件里更改，每次容器启动后机器人会自动启动
AutoStartBot="true"

```

​																

- [x] 如tg机器人给你发以下信息，证明你填写的机器人参数是正确的∆。

![图2：完成登录，tg机器人发通知](https://raw.githubusercontent.com/curtinlv/gd/main/img/p2.png)

- [x] 发送【user?】 给你的机器人，有以下回复，证明你通过号码成功授权登录。

![图3：测试1](https://raw.githubusercontent.com/curtinlv/gd/main/img/p3.png)

- [x] 在所监控的频道发出变量，机器人会马上通知：

![图4：测试2](https://raw.githubusercontent.com/curtinlv/gd/main/img/p4.png)



<h1 align="center">
  恭喜你，部署已完成。
  <br>
</h1>






```bash
#################### 相关命令 ####################
操作环境：进入容器内
## 查看机器人运行状态
pm2 status jbot

## 启动机器人：
pm2 start jbot

## 停止机器人
pm2 stop jbot

## 重启机器人
pm2 restart jbot

```



## 新增自定义监控配置文件 

- 2022.3.28 详见 conf/jk.json

```bash
首次更新方式：
1、以发送机器人命令方式：
# 下载自定义监控配置文件 jk.json
/cmd cd /ql/config && wget https://git.metauniverse-cn.com/https://raw.githubusercontent.com/curtinlv/gd/main/conf/jk.json
# 更新user.py
/cmd cd /ql/jbot/user && rm -f user.py  && wget https://git.metauniverse-cn.com/https://raw.githubusercontent.com/curtinlv/gd/main/user/user.py
# 更新 utils.py
/cmd cd /ql/jbot/diy && rm -f utils.py  && wget https://git.metauniverse-cn.com/https://raw.githubusercontent.com/curtinlv/gd/main/diy/utils.py
# 重启机器人生效
/restart

2.ssh进入容器方式：
# 下载自定义监控配置文件 jk.json
cd /ql/repo/gd && git pull  && cp -a /ql/repo/gd/conf/jk.json /ql/config
# 更新user.py
rm -f /ql/jbot/user/user.py && cp -a /ql/repo/gd/user/user.py /ql/jbot/user/user.py
# 更新 utils.py
rm -f /ql/jbot/diy/utils.py && cp -a /ql/repo/gd/diy/utils.py /ql/jbot/diy/utils.py
# 重启机器人生效
pm2 restart jbot


PS：后续只需修改 jk.json 配置文件，自己定义变量监控和应对执行脚本即可。修改完，需重启机器人生效。


```





# 特别感谢
- 脚本的写作参考了:
  - [SuMaiKaDe](https://github.com/SuMaiKaDe) 的 [bot](https://github.com/SuMaiKaDe/bot) 仓库
  - [chiupam](https://github.com/chiupam) 的 [JD_Diy](https://github.com/chiupam/JD_Diy) 仓库
  - [msechen](https://github.com/msechen) 的 [jdrain](https://github.com/msechen/jdrain) 仓库
  - 未完待定
