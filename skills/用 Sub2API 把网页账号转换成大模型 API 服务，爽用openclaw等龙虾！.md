把Claude，GPT，Gemini，Antigravity的网页订阅账号转换成标准的Api接口服务

![7d8905f7ad34b06090b0d2f08c4e5c4c.png](../_resources/7d8905f7ad34b06090b0d2f08c4e5c4c.png)

看做中转也可以，有的中转站也是套壳这个项目，可以共享额度和多账号切换

![bf2d11c450d316486929ae833f6f4cfd.png](../_resources/bf2d11c450d316486929ae833f6f4cfd.png)

原理是把ChatGPT账户通过授权方式反代出来，免费账号也足够使用

![5d8246a138250966e9742f67ed56ff27.png](../_resources/5d8246a138250966e9742f67ed56ff27.png)

这是我反代的ChatGPT账户额度Api

https://mmbiz.qpic.cn/mmbiz_png/xMvTmmstK21OoFJwTuvYFT20W40icjN0UBvPtDiayZJElXkJqDlaTicXD4ychem3mxAnShQicfG6rhYATIknHTnbzjbVbaxgVziaPgn1gwQkicdqU/640?wx_fmt=png&from=appmsg
项目地址：
https://github.com/Wei-Shaw/sub2api

要使用服务器部署，不要选择国内地区↓↓↓，选择linux版本，有Docker Compose容易
腾讯云新加坡，硅谷，东京地区价格是199元一年，2核4G30M带宽，60GBSSD盘 1.5T月流量

购买地址：https://curl.qcloud.com/oyWDLkRJ

![901e6a6b9d578895522b0f72d15243ec.png](../_resources/901e6a6b9d578895522b0f72d15243ec.png)

**部署教程**

1.我的系统是Debian，SSH连接服务器后，先安装仓库源

```
sudo apt update
sudo apt install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
sudo tee /etc/apt/sources.list.d/docker.sources > /dev/null <<EOF
Types: deb
URIs: https://download.docker.com/linux/debian
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF
```

![30356fed42590f807383e186f36f044b.png](../_resources/30356fed42590f807383e186f36f044b.png)

2.安装 Docker 与 Compose

```
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo systemctl enable docker --now
```

![4f96fd770060fab536b09e950b0f9ba8.png](../_resources/4f96fd770060fab536b09e950b0f9ba8.png)

3.安装sub2api

```
mkdir -p sub2api-deploy && cd sub2api-deploy
curl -sSL https://raw.githubusercontent.com/Wei-Shaw/sub2api/main/deploy/docker-deploy.sh | bash
docker compose up -d
docker compose logs -f sub2api
```

![2374b4c54542592f7669be6e1509593d.png](../_resources/2374b4c54542592f7669be6e1509593d.png)

4.用 http://你的服务器IP:8080 打开即可

![8201dbbf51df32be75f82ed785f31c14.png](../_resources/8201dbbf51df32be75f82ed785f31c14.png)
5.登录

管理员用户名是admin@sub2api.local
密码是容器首次启动时自动生成的一次性管理员密码，这时用下面命令找
```
cd ~/sub2api-deploy
docker compose logs sub2api | grep -i admin
```

![4036ff65f3d1e4e2a02a31e1a9941ec5.png](../_resources/4036ff65f3d1e4e2a02a31e1a9941ec5.png)

6.点击账号管理，添加ChatGPT网页账号，账号类型选OAuth

![5fb87c2240f132cbd64b067a99d16606.png](../_resources/5fb87c2240f132cbd64b067a99d16606.png)

生成授权链接

![ce256df0ef2130bc75b3769a03bdcb83.png](../_resources/ce256df0ef2130bc75b3769a03bdcb83.png)

在浏览器中打开链接并完成授权

![8aaadd616c86a10315e6ae8e5e2b406a.png](../_resources/8aaadd616c86a10315e6ae8e5e2b406a.png)

输入授权链接或 Code
授权完成后，当页面地址变为 http://localhost:xxx/auth/callback?code=... 时：完整复制进去会自动识别

![faf54e97d92c63a91d2bc6815084e92f.png](../_resources/faf54e97d92c63a91d2bc6815084e92f.png)

成功，可以愉快使用ChatGPT的Api了

![fe31a1331b740372fa6f4a38f23e60cd.png](../_resources/fe31a1331b740372fa6f4a38f23e60cd.png)