# 开始开发ScratchHW

## 开发环境

确保你的计算机中安装有以下工具及软件。

- Node 8
- python
- Git

**注意**: 如果你在中国，你需要使用代理软件来为npm下载的内容提供加速，否则有很大概率会下载失败。

## Clone & Install

1. Clone 所有必须的仓库。 (使用```--depth``` 指令来减小下载文件的大小，否则gui的文件体积将高达1.18GB)

   ```bash
   mkdir ScratchHW
   cd ScratchHW
   git clone https://github.com/ScratchHW/saveSvgAsPng
   git clone --depth 3 https://github.com/ScratchHW/scratch-gui
   git clone --depth 3 https://github.com/ScratchHW/scratch-blocks
   git clone https://github.com/ScratchHW/scratch-l10n
   git clone --depth 3 https://github.com/ScratchHW/scratch-vm
   git clone https://github.com/ScratchHW/scratch-link
   git clone https://github.com/ScratchHW/scratch-extension-server
   ```

2. Install 和 link

   ```bash
   cd saveSvgAsPng
   npm install
   npm link
   cd ..
   cd scratch-blocks
   npm install
   npm link
   cd ..
   cd scratch-l10n
   npm install
   npm link
   cd ..
   cd scratch-vm
   npm install
   npm link
   cd ..
   cd scratch-link
   npm install
   npm link
   cd ..
   cd scratch-extension-server
   npm install
   npm link
   cd ..
   cd scratch-gui
   npm install
   cd ..
   ```

## 运行

1. 启动 scratch-link

   ```bash
   cd scratch-link
   npm start
   ```

2. 启动 scratch-extension-server

    开启一个新的终端。

    ```
    cd scratch-extension-server
    npm start
    ```

3. 启动 scratch-gui

   开启一个新的终端。

   ```
   cd scratch-gui
   npm run start-open
   ```

4. 在 webpack 打包完成后，你会就会看到一个浏览器窗口弹出来了，体验一下吧~。