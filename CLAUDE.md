# Faraway's Way — 個人 Portfolio 網站

## 專案背景

這是 Jack（Yuan Jack Yao，姚遠）的個人 Portfolio 網站，網址為
`starsexpress.github.io/Faraway-s-Way`。

- 框架：Hugo + Hextra theme（基於 Hextra Starter Template）
- 部署：GitHub Pages，純靜態，不碰額外配置
- 目標上線日：2026/6/30
- Hugo 版本：v0.163.2+extended（需要 extended 版本以支援 Sass 編譯）
- 需要 Go 環境（Hugo Modules 依賴管理）


## 頂部導覽列順序與圖示（從左到右，固定順序，不要打亂）

```
🙉MonkeyJOB🎰   🪂SkyHorse🐎   📕Notes✏️   💼CV🧑🏻‍💻   |   🏔️Faraway Life🌊
```

- 前四項（MonkeyJOB / SkyHorse / Notes / CV）是正式作品與技術內容，圖示走專業/工具感路線
- Faraway Life 是生活/旅遊內容，圖示風格刻意不同（山+海，呼應「上山下海」與「Faraway」雙關），與前四項之間建議用視覺分隔線（divider）區隔，暗示性質不同
- 排序邏輯：作品先說話（MonkeyJOB/SkyHorse）→ 技術思考次之（Notes）→ CV 收尾給行動指令 → Faraway Life 放最後當彩蛋，不搶主線焦點

## 頂層結構（6 個頂層分區：5 個主分區 + 首頁）

```
content/
├── _index.md          → 首頁，同時作為「自我介紹」頁
│                          上方：個人照（已選定：深色襯衫+西裝外套、抱手、自然光、綠色背景的那張，
│                            表情自信放鬆，背景簡潔不搶焦，適合裁切成圓形/方形頭像）
│                            + GitHub/LinkedIn 按鈕
│                          中段：文字自我介紹
│                          中區：作品 Cards（帶縮圖、可點擊跳轉），目前規劃三張：
│                            - MonkeyJOB → 連到 /monkeyjob/
│                            - SkyHorse → 連到 /skyhorse/
│                            - Open Source Contributions → 連到 /notes/leetcode-contributions/
│                          （Cards 連去的是站內介紹頁，不是直接連外部連結）
├── monkeyjob/
│   └── _index.md      → 🙉🎰 JOB: Jack's Online Blackjack 介紹頁
│                          內容：為何做這個專案、技術棧（PyWebIO → FastAPI + vanilla JS 重構故事）、
│                          跳轉連結到 https://jack-s-onlineblackjack.up.railway.app/
├── skyhorse/
│   └── _index.md      → 🪂🐎 天码行空 · SkyHorse 介紹頁
│                          內容：LeetCode 題解定位、Docusaurus + 雙語、doocs/leetcode 貢獻經歷、
│                          跳轉連結到 https://starsexpress.github.io/SkyHorse/
├── notes/              → 📕✏️ Technical Notes（原創技術解讀，不是教科書複述）
│   ├── _index.md
│   ├── lstm.md         → LSTM 各公式設計解讀
│   ├── bayes.md        → Bayes Theorem 解讀
│   └── leetcode-contributions.md → Open Source Contributions（見下方專節說明）
├── cv/                 → 💼🧑🏻‍💻 CV（原規劃稱為 Contact，現定名 CV）
│   └── _index.md       → 結構：
│                          1. 按鈕區（最上方）：[📄 Download Résumé] [GitHub ↗] [LinkedIn ↗] [✉️ Email]
│                          2. Experience 摘要（Walmart Gen AI 實習、SUEZ Shanghai DS）
│                          3. Education（Georgia Tech MS Analytics, 4.0 GPA, Dec 2026）
│                          4. Techniques（見下方專節說明，icon grid 呈現）
└── faraway-life/        → 🏔️🌊 旅遊/生活內容，與前四項視覺上做區隔
    └── _index.md        → 上海工作四年期間走訪各城市的精選旅遊照
                             定位：軟性加分項，展現生活面/視野，不是完整遊記
                             原則：精選克制，幾張照片 + 地點 + 一兩句話即可，不要做成相冊式長頁
```

**重要：不要把 MonkeyJOB 和 SkyHorse 做成導覽列裡的外部連結直接跳轉。**
點進去要先進到一個獨立介紹頁（說明專案動機、技術棧），頁面裡再放跳轉按鈕到實際的 demo/repo。

## CV 頁面 Techniques 區規劃（icon grid 呈現，仿照常見技術棧展示風格）

```
🪄 Gen AI & ML
   Python

🚀 Automation & CI/CD
   GitHub Actions · Git

☁️ Cloud & Data Platforms
   AWS · Azure · GCP

🗄️ Databases
   MySQL · PostgreSQL

👀 Frontend & Backend
   JavaScript · FastAPI · C++
```

這份清單是初版定稿，之後若要補框架細節（TensorFlow/PyTorch/Hugging Face 等）或調整分類，
之後再行更新，不要自行假設或補空。圖示來源可用 Simple Icons 或類似 icon CDN。
