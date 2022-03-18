```  
name: 打包应用并上传腾讯云

on:
    push:
        branches:
            - master

jobs:
    build:
        # runs-on 指定job任务运行所需要的虚拟机环境(必填字段)
        runs-on: ubuntu-latest
        steps:
            # 获取源码
            - name: 迁出代码
              # 使用action库  actions/checkout获取源码
              uses: actions/checkout@master

            # 安装Node10
            - name: 安装node.js
              # 使用action库  actions/setup-node安装node
              uses: actions/setup-node@v1
              with:
                  node-version: 14.0.0

            # 安装依赖
            - name: 安装依赖
              run: npm install

            # 打包
            - name: 打包
              run: npm run build

            # 上传腾讯云
            - name: 发布到腾讯云
              uses: easingthemes/ssh-deploy@v2.1.1
              env:
                  # 私钥
                  SSH_PRIVATE_KEY: ${{ secrets.PRIVATE_KEY }}
                  # scp参数
                  ARGS: '-avzr --delete'
                  # 源目录
                  SOURCE: 'dist'
                  # 服务器ip：换成你的服务器IP
                  REMOTE_HOST: ${{ secrets.REMOTE_HOST }}
                  # 用户
                  REMOTE_USER: 'root'
                  # 目标地址
                  TARGET: '/www/wwwroot'  
                  
```  
