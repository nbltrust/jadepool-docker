### saas-hub-docker使用说明

#### 一.PRECONDITIONS：
1. 服务器需先装好docker及docker-compose(建议version 1.24.0)。docker请自行安装，下面提供了docker-compose的安装步骤：
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose --version
```
2. 用我们给出的阿里云账号登录 http://signin.aliyun.com/nbltrust/login.htm,并设置Registry密码,然后在服务器执行以下登录命令:
```bash
docker login --username=用户名@nbltrust registry.cn-hangzhou.aliyuncs.com      // 用刚设置的Registry密码登录,一台服务器只需登录一次
```

#### 二.步骤：
1. 准备数据：
```bash
git clone https://github.com/nbltrust/jadepool-docker-scripts.git
cp -rf ./jadepool-docker-scripts/data /
```
如需个性化设置，比如设置seed密码,可以修改init.json; 修改冷钱包地址,可以修改coldwallet.json：
```bash
cd /data
cd seed-app
vim init.json          // 设置密码
vim coldwallet.json    // 设置冷钱包地址
./main.sh              // 执行进行初始化
```

2. 修改saas-backend配置
需要用户按需修改saas-backend配置
```bash
vim ./dev.yaml

# 修改sms短信服务
sms:
  china:
    app_id: 12345
    app_key: xxxxx292893dd6bf0d3baac96a71858d
  internation:
    app_id: 12345
    app_key: xxxxx176ad40bb5c75d5a761aac2855e
  zhSignature: 瑶池
  enSignature: Jadepool

# 修改saas前端url, 将下面127.0.0.1改为当前机器ip
saasadmin:
  saas_web_url: "http://127.0.0.1:3000"
```

2. 调整docker-compose.yml中的使用的镜像版本号及模式等后，启动容器:

```bash
docker-compose up -d            // docker compose的方式启动容器
```

3. 启动后等待1分钟后即可在浏览器中访问以下服务:
```
host:3000 hub admin前端        -> 初次登陆会初始化hub superadmin账号
host:3003 saas frontend前端
host:3009 saas admin前端       -> 初次登陆会初始化saas superadmin账号
```
