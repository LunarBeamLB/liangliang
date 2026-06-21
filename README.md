# 参考链接
* 官方文档：[quartz](https://quartz.jzhao.xyz/)
* 安装参考：[使用Obisidian和Quartz 4编写博客](https://ningh.net/articles/blog-with-obsidian-quartz4) [Quartz 教程：从零开始构建你的数字花园](https://ainightcoder.github.io/8.%E8%BE%93%E5%87%BA/quartz/0d98412b93ef972b5389388471dcb61e1e6e479c)
* systemd服务参考：[终极Bash脚本服务编排指南：systemd与bash完美结合教程](https://blog.csdn.net/gitblog_00667/article/details/153857541#:~:text=%E6%9C%AC%E6%96%87%E5%B0%86%E6%95%99%E4%BD%A0%E5%A6%82%E4%BD%95%E9%80%9A%E8%BF%87%20%2A%2Asystemd%2A%2A%20%E5%B0%86Bash%E8%84%9A%E6%9C%AC%E8%BD%AC%E5%8C%96%E4%B8%BA%E7%A8%B3%E5%AE%9A%E7%9A%84%E7%B3%BB%E7%BB%9F%E6%9C%8D%E5%8A%A1%EF%BC%8C%E5%AE%9E%E7%8E%B0%E8%87%AA%E5%8A%A8%E5%8C%96%E8%BF%90%E8%A1%8C%E4%B8%8E%E7%9B%91%E6%8E%A7%E3%80%82%20%23%23%20%F0%9F%9A%80%20%E4%B8%BA%E4%BB%80%E4%B9%88%E9%80%89%E6%8B%A9systemd%E7%AE%A1%E7%90%86Bash%E8%84%9A%E6%9C%AC%EF%BC%9F,-%20%2A%2A%E5%BC%80%E6%9C%BA%E8%87%AA%E5%90%AF%2A%2A%EF%BC%9A%E6%97%A0%E9%9C%80%E6%89%8B%E5%8A%A8%E8%BF%90%E8%A1%8C%EF%BC%8C%E7%B3%BB%E7%BB%9F%E5%90%AF%E5%8A%A8%E6%97%B6%E8%87%AA%E5%8A%A8%E6%BF%80%E6%B4%BB%E6%9C%8D%E5%8A%A1-%20%2A%2A%E8%BF%9B%E7%A8%8B%E5%AE%88%E6%8A%A4%2A%2A%EF%BC%9A%E8%84%9A%E6%9C%AC%E5%B4%A9%E6%BA%83%E6%97%B6%E8%87%AA%E5%8A%A8%E9%87%8D%E5%90%AF%EF%BC%8C%E7%A1%AE%E4%BF%9D%E6%9C%8D%E5%8A%A1%E7%A8%B3%E5%AE%9A%E6%80%A7-%20%2A%2A%E6%97%A5%E5%BF%97%E9%9B%86%E6%88%90%2A_systemd%20%E6%89%A7%E8%A1%8C%20bash%E5%91%BD%E4%BB%A4)
* github加速：[jasonzeng](https://gh.jasonzeng.dev/)
# 前置配置
## 安装 Node
* 在[node官网](https://nodejs.org/en/download)复制命令在终端中用root用户执行。
* 查看安装：
```bash
node -v && npm -v
```
## 安装git
* 在终端中用root用户执行：
```bash
sudo apt update  
sudo apt install git -y
```
* 查看安装：
```bash
git -v
```
# 配置github代理加速
* 以jasonzeng镜像为例。
## 配置过程
1. 在终端以root用户输入指令：
```bash
git config --global url."https://gh.xmly.dev/https://github.com/".insteadOf https://github.com/
```
## 配置github全局镜像命令
* 将 GitHub 的 URL 替换为自定义镜像
```bash
git config --global url."https://gh.xmly.dev/https://github.com/".insteadOf https://github.com/
```
* 删除自定义镜像。需要填入对应的url
```bash
git config --global --unset url."https://gh.xmly.dev/https://github.com/".insteadOf
```
* 查看设置的自定义镜像
```bash
git config --list --show-origin
```
# 创建 Quartz 项目
1. 克隆 Quartz 仓库，并跳转到quartz文件夹内
```bash
git clone https://github.com/jackyzha0/quartz.git
cd quartz
```
# 以systemd服务的形式运行
## 配置过程
1. 创建shell脚本。用文本编辑器新建`quartz.sh`文件保存到一个自定义位置：`/root/default/shell/quartz.sh`，添加内容：
```bash
#!/bin/bash
# 加载系统全局环境 + 用户环境
source /etc/profile
source ~/.bashrc
# 跳转的quarz安装目录
cd /vol1/1000/default/quartz
# 启动；端口设置为33090
npx quartz build --serve --port 33090
```
2. 在终端中，以root用户赋予脚本执行权限，输入命令：
```bash
chmod +x /root/default/shell/quartz.sh
```
3. 创建systemd服务。用文本编辑器新建`quartz.service`文件保存到`/etc/systemd/system/quartz.service`，添加内容：
```ini
[Unit]
Description=My Custom Script Service
After=network.target
[Service]
# shell脚本保存的路径
ExecStart=/root/default/shell/quartz.sh
Restart=on-failure
User=root
[Install]
WantedBy=multi-user.target
```
4. 运行systemd服务。在终端中用root用户运行命令：
```bash
# 重载配置
sudo systemctl daemon-reload
# 启动服务
sudo systemctl start quartz.service
# 设置开机启动
sudo systemctl enable quartz.service
```
## systemd命令
* 重新加载 systemd 配置
```bash
sudo systemctl daemon-reload
```
* 启动服务
```bash
sudo systemctl start quartz.service
```
* 查看状态
```bash
sudo systemctl status quartz.service
```
* 停止服务
```bash
sudo systemctl stop quartz.service
```
* 设置开机自启
```bash
sudo systemctl enable quartz.service
```
* 关闭开机自启
```bash
sudo systemctl disable quartz.service
```

# 修改配置
* 修改配置 quartz.config.yaml
	* 中文 locale: zh-CN
	* 设置url，这项决定了qq链接小卡片是否正常。baseUrl: ob.guac.cn
	* 设置域名备案
```yaml
  - source: github:quartz-community/footer
    enabled: true
    options:
      links:
        桂公网安备45032602000060号: https://beian.mps.gov.cn/#/query/webSearch?recordcode=45032602000060
        桂ICP备2026009669号-1: https://beian.miit.gov.cn/
```
 
```bash
bash/root/default/shell/quartz.sh
```
