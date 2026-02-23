# Attention-Restoration-Code-Collection
Short text blocker
Including webpage plugins or some app 
I will continue exploring this fields

# Zhihu High-Quality Reader (知乎首页高质量阅读器)

一个轻量级的 Tampermonkey 用户脚本，用于自动过滤知乎推荐流（首页、关注、热榜）中的低质量短回答。

## ⚙️ 过滤逻辑

脚本基于卡片的前端 DOM 元素，采用三步判定机制拦截内容。被拦截的卡片不会被直接删除（避免破坏瀑布流引发无限刷新），而是替换为占位提示条。

1. **检测「阅读全文」按钮（折叠状态）**
   - **未折叠**：提取全文纯文本字数与 `<figure>` 图片数。若 `字数 + (图片数 × 50) < 300`，判定为短文，执行**过滤**。
   - **已折叠**：进入下一步。

2. **检测「封面配图」**
   - **有配图**：判定为图文长文，**放行**。
   - **无配图**：进入下一步。

3. **检测「摘要」长度**
   - 提取显示的摘要纯文本字数。若 `< 60 字`，判定为低质量截断，执行**过滤**；否则**放行**。

## 🚀 安装与使用

1. 安装浏览器脚本管理器，如 [Tampermonkey](https://www.tampermonkey.net/)。
2. 添加新脚本，将本项目 `script.js` 的源码粘贴并保存。
3. 打开或刷新知乎首页 (`https://www.zhihu.com/`) 即可生效。

## 🛠️ 自定义配置

脚本顶部提供常量配置区，可自行修改判定阈值：

```javascript
const FULLTEXT_MIN_SCORE = 300;  // 完整未折叠文章的最低放行得分
const IMAGE_WEIGHT = 50;         // 单张配图折算的字数权重
const SUMMARY_MIN_LENGTH = 60;   // 无图折叠文章的摘要最低放行字数