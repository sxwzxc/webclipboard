## 在线剪切板

## 开发和部署指南

### 如果修改了项目代码，如何运行和部署？

本项目有三种部署方式，根据你修改的内容选择相应的方式：

#### 1. 修改了 allnode_version 目录的代码（Node.js 版本）

**本地开发运行：**
```bash
# 进入项目目录
cd webclipboard/allnode_version

# 安装依赖（首次运行或 package.json 有变化时）
npm install

# 启动开发服务器
node api.js

# 访问 http://localhost:3000 进行测试
```

**部署到生产环境：**
- **使用源码部署**：参考下方"源码部署"章节，重新执行部署步骤
- **使用 Docker 部署**：需要重新构建镜像
  ```bash
  cd allnode_version
  docker build -t webclipboard-custom .
  docker run -dp 8080:3000 webclipboard-custom
  ```

#### 2. 修改了 cf_version 目录的代码（Cloudflare Workers 版本）

**本地开发运行：**
```bash
# 进入 cf_version 目录
cd webclipboard/cf_version

# 安装依赖（首次运行或 package.json 有变化时）
npm install

# 本地开发模式（可选）
npx wrangler dev

# 部署到 Cloudflare
npx wrangler login  # 首次需要登录
npx wrangler deploy
```

**注意**：如果修改了前端文件（如 `allnode_version/public/` 下的 HTML 文件），需要重新上传到 R2：
```bash
cd cf_version
npx wrangler r2 object put webclipboard-storage/public/index.html --file=../allnode_version/public/index.html
npx wrangler r2 object put webclipboard-storage/public/t2.html --file=../allnode_version/public/t2.html
```

#### 3. 修改了根目录的独立工具（daily-note.js, pic-viewer.js）

这些是独立的 HTML/JavaScript 文件，可以：
- 直接在浏览器中打开 HTML 文件进行测试
- 部署到任何静态文件服务器（如 GitHub Pages、Vercel、Netlify 等）

---

<details>
<summary>源码部署(点击展开)</summary>

1. git clone 本项目到服务器
2. 进入 `allnode_version/` 目录下
3. 安装 node 环境 
4. 执行 `npm install` 安装依赖
3. 使用命令 `node api.js` 启动服务
4. 访问 `http://<你的服务器ip>:3000` 即可使用剪切版

```SH
#!/bin/bash
# 1. git clone 本项目到服务器
git clone https://github.com/cornradio/webclipboard

# 进入 `allnode_version/` 目录下
cd webclipboard/allnode_version

# 检查 Node.js 是否已经安装
if ! command -v node &> /dev/null; then
    echo "Node.js 未安装，请先安装 Node.js"
    exit 1
fi

# 4. 执行 `npm install` 安装依赖
npm install

# 5. 使用命令 `node api.js` 启动服务
node api.js &  # 使用 & 让服务在后台运行

# 6. 访问 `http://<你的服务器ip>:3000` 即可使用剪切版
echo "服务已启动，请访问 http://<你的服务器ip>:3000 使用剪切版"
```
</details>


docker部署：参考 [allnode_version](allnode_version/README.md)  
cf workers部署：参考 [cf_version](cf_version/README.md)



## 截图
<div align="center">
  <table>
    <tr>
      <td width="70%">
        <img src="https://github.com/user-attachments/assets/f489f649-71de-4ff1-bfff-adadece727c5" alt="Image 1">
      </td>
      <td width="30%">
        <img src="images/image2.jpg" alt="Editor">
      </td>
    </tr>
  </table>
</div>

<div align="center">
  <img src="https://github.com/user-attachments/assets/ba174d9c-311a-4b44-8171-61a7a4c71aef" alt="Image 3" width="800">
</div>

## 历史记录功能
- 使用 `localStorage` 保存本地访问过的文件名历史记录
- 通过鼠标移动到网页左上角的方式查看历史记录
  
![历史记录](https://raw.githubusercontent.com/cornradio/imgs/main/blog/Clip_2024-07-17_19-47-01.png)


## 命令行使用（curl命令）

`uploadtxt.sh` 可以上传 `a.json` 文件到服务器 `1.txt` 文件中
```shell
curl \
-X POST http://<your-ip>/api/writefile/1.txt \
-H "Content-Type: application/json" \
-d "{\"content\": \"$(awk '{printf "%s\\n", $0}' a.json | sed 's/"/\\"/g')\"}"
```

## 类似tool
https://fagedongxi.com/  
https://getnote.top/  
https://hackmd.io/?nav=overview

## 更新日志
### 2026-01-29
- 增加了图片、日记插件。（可以选择关闭）
- 增加了全屏笔记功能。（快捷键 F）
- 增加了快速跳转到文件名功能（快捷键 T）

<img width="2054" height="1335" alt="Image" src="https://github.com/user-attachments/assets/3cbd05ce-f568-4843-b006-6a7139ee8471" />


