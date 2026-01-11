# 五子棋游戏

一个使用 Vue 3 + Vite 构建的经典五子棋游戏，支持双人对战。

## 技术栈

- **Vue 3** - 使用 Composition API 和 `<script setup>` 语法
- **Vite** - 新一代前端构建工具，提供极速的开发体验
- **CSS** - 使用 scoped 样式和渐变效果

## 功能特性

- 15x15 标准棋盘
- 双人对战模式（黑棋先行）
- 实时显示当前执棋方
- 五子连珠胜利检测（横、竖、斜方向）
- 最后落子位置标记
- 游戏结束弹窗提示
- 一键重新开始

## 快速开始

### 安装依赖

```bash
npm install
```

### 开发模式

```bash
npm run dev
```

访问 http://localhost:3000 即可开始游戏。

### 构建生产版本

```bash
npm run build
```

构建产物将输出到 `dist` 目录。

### 预览生产构建

```bash
npm run preview
```

## 游戏规则

1. 黑棋先行，双方轮流落子
2. 点击棋盘交叉点进行落子
3. 先在横、竖、斜任意方向连成五子者获胜
4. 游戏结束后可点击重新开始

## 项目结构

```
wuziqi-game/
├── index.html              # HTML 入口文件
├── vite.config.js          # Vite 配置文件
├── package.json            # 项目依赖配置
└── src/
    ├── main.js             # 应用入口
    ├── App.vue             # 根组件
    ├── components/
    │   └── WuziqiBoard.vue # 棋盘组件（包含游戏逻辑）
    └── styles/
        └── main.css        # 全局样式
```

## 开发说明

- 所有代码注释使用中文
- 使用 Vue 3 响应式 API 管理游戏状态
- 棋盘使用 CSS Grid 布局实现
- 胜利检测采用方向向量算法

## License

MIT
