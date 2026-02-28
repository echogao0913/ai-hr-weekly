# AI在HR领域的每周应用 - 使用说明

## 📋 项目简介

这是一个展示AI在人力资源各领域应用的网页应用，每周更新最新的AI+HR资讯。

**涵盖领域：**
- 💰 薪酬福利
- 🎓 人才发展
- 🏢 组织发展
- 🎨 企业文化
- ⚙️ SSC共享服务

## 📁 项目文件

```
ai_hr_weekly.html        # 主页面（现代卡片式布局）
hr_news_scraper.py       # Python爬虫脚本（自动抓取新闻）
hr_news_data.json        # 新闻数据文件（JSON格式）
README_AI_HR.md          # 使用说明（本文件）
```

## 🚀 快速开始

### 1. 本地查看

直接用浏览器打开 `ai_hr_weekly.html` 文件即可查看。

### 2. 更新数据

运行Python脚本更新新闻数据：

```bash
python hr_news_scraper.py
```

脚本会自动：
- 生成示例数据（10条AI+HR新闻）
- 按周分组和按类别分类
- 保存到 `hr_news_data.json`

### 3. 刷新页面

更新数据后，刷新浏览器页面即可看到最新内容。

## 🔧 自定义配置

### 手动编辑数据

打开 `hr_news_data.json`，按以下格式添加新闻：

```json
{
  "title": "新闻标题",
  "description": "新闻描述",
  "category": "薪酬福利",  // 或: 人才发展、组织发展、企业文化、SSC
  "source": "来源网站",
  "link": "https://example.com/article",
  "week": "2026年第9周",
  "date": "2026年02月23日"
}
```

### 修改网页样式

编辑 `ai_hr_weekly.html` 中的 CSS 样式：
- 修改颜色：查找 `#667eea` 和 `#764ba2`（主题色）
- 修改字体：查找 `font-family`
- 修改布局：查找 `.cards-grid` 的 grid 设置

## 🌐 发布到GitHub Pages

### 步骤1：创建GitHub仓库

```bash
# 初始化git仓库
git init

# 添加文件
git add ai_hr_weekly.html hr_news_data.json hr_news_scraper.py README_AI_HR.md

# 提交
git commit -m "Initial commit: AI in HR Weekly Application"

# 关联远程仓库（替换为你的仓库地址）
git remote add origin https://github.com/你的用户名/ai-hr-weekly.git

# 推送到GitHub
git push -u origin master
```

### 步骤2：启用GitHub Pages

1. 进入GitHub仓库页面
2. 点击 **Settings** > **Pages**
3. Source 选择 **main** 或 **master** 分支
4. 保存后等待几分钟

### 步骤3：访问网站

网站地址：`https://你的用户名.github.io/ai-hr-weekly/ai_hr_weekly.html`

## 🔄 每周自动更新（可选）

### 方案1：GitHub Actions（推荐）

创建 `.github/workflows/update-news.yml`：

```yaml
name: Update HR News

on:
  schedule:
    # 每周一早上8点运行
    - cron: '0 0 * * 1'
  workflow_dispatch:

jobs:
  update:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.10'

      - name: Install dependencies
        run: |
          pip install requests beautifulsoup4

      - name: Run scraper
        run: python hr_news_scraper.py

      - name: Commit changes
        run: |
          git config --local user.email "action@github.com"
          git config --local user.name "GitHub Action"
          git add hr_news_data.json
          git commit -m "Auto update: $(date +'%Y-%m-%d')" || echo "No changes"
          git push
```

### 方案2：Windows任务计划

1. 打开"任务计划程序"
2. 创建基本任务
3. 触发器：每周一早上8点
4. 操作：启动程序
   - 程序：`python`
   - 参数：`C:\Users\gaohanqi\hr_news_scraper.py`
5. 完成设置

## 🛠 高级功能

### 实现真实爬虫

在 `hr_news_scraper.py` 中添加具体网站的爬取逻辑：

```python
def scrape_hrbar(self):
    """抓取HR Bar新闻"""
    url = "https://www.hrbar.com/news"
    response = requests.get(url, headers=self.headers)
    soup = BeautifulSoup(response.text, 'html.parser')

    # 根据网站结构提取信息
    articles = soup.find_all('article', class_='news-item')
    for article in articles:
        title = article.find('h2').text
        link = article.find('a')['href']
        # ... 提取其他信息

        if self.is_ai_related(title):
            category = self.categorize_content(title, '')
            self.news_data.append({
                'title': title,
                'link': link,
                'category': category,
                # ...
            })
```

### 使用RSS订阅

安装 feedparser：
```bash
pip install feedparser
```

添加RSS解析代码：
```python
import feedparser

def scrape_rss(self, rss_url):
    feed = feedparser.parse(rss_url)
    for entry in feed.entries:
        if self.is_ai_related(entry.title):
            # 处理条目...
```

### 使用API接口

常用的新闻API：
- NewsAPI: https://newsapi.org/
- 聚合数据: https://www.juhe.cn/
- 天聚数据: https://www.tianapi.com/

## 📊 数据源推荐

### 国内HR资讯网站
1. **HRoot** (https://www.hroot.com/)
2. **人力资源管理网** (https://www.hr.com.cn/)
3. **三茅人力资源网** (https://www.hrloo.com/)
4. **HR沙龙** (https://www.hrbar.com/)

### 科技媒体HR频道
1. **36氪** - HR科技频道
2. **虎嗅** - 商业科技
3. **钛媒体** - 企业服务

### 微信公众号
1. HRTechChina
2. 人力资源智享会
3. HR科技云图

## ⚠️ 注意事项

1. **爬虫合规性**
   - 遵守网站 robots.txt
   - 控制请求频率
   - 不要用于商业用途

2. **反爬虫应对**
   - 使用代理IP池
   - 添加随机延迟
   - 模拟浏览器行为

3. **数据质量**
   - 定期检查数据准确性
   - 过滤无效链接
   - 去重处理

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 📝 许可

MIT License

## 📧 联系方式

如有问题或建议，请通过以下方式联系：
- GitHub Issues
- Email: your-email@example.com

---

**最后更新：** 2026年2月
**版本：** v1.0.0
