---
background: https://images.unsplash.com/photo-1625082688687-e4b9d610652f
class: text-center
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
title: FE Sharing 2025.03.27
mdc: true
monaco: true
---

# Sharing 2025.03.27

Jonathan

---
layout: image-right

# the image source
image: /20250327/phoenix-project-cover.jpg
backgroundSize: 50%

---

# 鳳凰專案
- 出版於 2013
- 虛構小說
- 從 IT 鬼故事看軟體開發與維運

---
layout: center
--- 

# 故事是這樣的

---
layout: center
--- 

# 主角 Bill
- 無極限零件公司的 IT 部門小主管
- 被告知他上面的兩位主管都請辭走人了
- 原地提拔成為 IT 部門主管

---
layout: center
--- 

# 鳳凰專案
- 公司線下銷售轉線上銷售的電商專案
- 行銷部門可以在後台更精準的掌握銷售狀況
- 產品部門可以在後台查看庫存、供應鏈系統以利原料採購、開發新商品

---
layout: center
--- 

# 鳳凰專案有什麼問題
- 開發時程落後一年以上
- 爆預算
- 充滿 bug 與版本衝突

---
layout: center
--- 

# 開發部 > IT 部 > QA 部

---
layout: center
--- 

# Bill 馬上遇到的問題
- 明天發薪的計算結果跑不出來，準備被業務工會告
- 資安部提出 100 多項的安全問題，要滿足三天後的外部稽核
- 其他 bug 等著要修正
- 其他功能等著要上線
- 鳳凰專案七天後要上線，出事就是整個部門裁撤並外包

---
layout: center
--- 

# 董事會成員 Erik
- 藉由帶 Bill 逛生產工廠導出管理概念
- 軟體開發 vs 工廠生產線

---
layout: center
---

# 書中提到的概念
- <Link to="process-improvement" title="流程"/>
- <Link to="half-finished-product-crisis" title="半成品危機"/>
- <Link to="four-types-of-work" title="四種工作類型"/>
- <Link to="constraint-point-theory" title="約束點理論"/>
- <Link to="wait-time-vs-utilization" title="需求等待時間"/>
- <Link to="three-step-work-method" title="三步工作法"/>

---
routeAlias: process-improvement
layout: center
---

# 流程

<v-click>

- 產線表現不如預期，往往想要改良流程來達到目的

</v-click>

<v-click>

- 流程的目的是讓工作更順利，而不是增加困難

</v-click>

<v-click>

- 流程的調整要能讓參與者了解
  - 調整後能解決的問題
  - 調整後帶來的效益

</v-click>

<v-click>

- 參與者願意投入的流程，才是實際存在的流程

</v-click>
---
routeAlias: half-finished-product-crisis
layout: center
---

# 半成品危機 - 1

- 以工廠來說，原料進廠直到送達客戶手中之前，任何步驟後的產物都只算是半成品

<v-click>

- 半成品如果無法出廠，就會造成隱性損失
  - 倉儲成本
  - 人工成本
  - 時間成本

</v-click>

---
layout: center
---

# 半成品危機 - 2

- 軟體開發也是一樣
  - 需求進入開發後，直到交付給使用者之前，都是半成品
  - 就算已經開發完成，未經測試、未經部署，一樣還是半成品

<v-click>

- 無法完成交付，也會造成隱性損失
  - 倉儲成本
  - 人工成本
  - 時間成本

</v-click>
---
layout: center
---

# 半成品危機 - 3

- 以工廠為例，半成品在廠內的停留時間越久...
  - 代表佔用資源越久
  - 交期拉長等於生產力變低

<v-click>

- 目標是提高系統生產力，而非提高任務完成的數量

</v-click>

<v-click>

- 解決半成品卡死產線的方法
  - 停止進料
  - 停止發布新訂單

</v-click>

---
layout: center
---

# 半成品危機 - 4

- 以軟體開發來說，便是需要有效的控管需求進入開發的數量
  - 防止非必要的需求進入開發
  - 將需求排入開發
  - 前者比後者更為重要

---
routeAlias: four-types-of-work
layout: center
---

# 四種工作類型
- 外部需求
- 內部需求
- 變更
- 計劃外的工作

---
layout: two-cols-header
--- 

# 對軟體開發而言

::left::

<v-clicks>

## 外部需求
- 產品新功能開發
- 產品功能改良、改版

## 內部需求
- 公司內部用工具
- 團隊內部用工具

</v-clicks>

::right::

<v-clicks>

## 變更
- 發 Pull Request
- 解決衝突
- Migration

## 計劃外的工作 = 反工作
- Hotfix
- 隕石

</v-clicks>

<style>
.two-cols-header {
  grid-template-rows: none !important;
}
</style>

---
layout: center
---

# 目前手上有什麼工作分類？
- 了解每類工作占用時間的比例
- 明確意識到計劃外工作的存在
- 四種工作類型的資源分配比重

---
routeAlias: constraint-point-theory
layout: center
---

# 約束點理論 - 1
- 約束點、瓶頸往往決定該單位整體能產出的量
- 以工廠為例，人員、原物料、機器都可以是約束點
- 以化學來說，約束點理論就是速率控制步驟 (rate-determining step)

---
layout: center
---

# 約束點理論 - 2
- 超級工程師的喜與悲
  - 喜：什麼問題他都可以輕鬆解決
  - 悲：所有問題只能靠他才能解決

<v-click>

- 約束點往往決定單位整體能產出的量

</v-click>

<v-click>

- 在約束點之外所做的任何改良都只是裝忙

</v-click>

---
layout: center
---

# 約束點理論 - 3
- 藉由約束點理論提升產能
  - 找出約束點
  - 保護約束點
  - 配合約束點
  - 解放約束點
  - 尋找新的約束點

---
routeAlias: wait-time-vs-utilization
layout: center
---

# 需求等待時間
- 若所有需求的優先度都相同
- 等待時間 = 開發人員忙碌百分比 / 開發人員閒置百分比

<v-click>

<img src="/20250327/wait-time-vs-utilization.png" class="m-10 h-60" />

</v-click>




---
routeAlias: three-step-work-method
layout: center
---

# 三步工作法

---
layout: center
---

# 第一步 - 系統思維

<img src="/20250327/first-way2.png.webp" class="m-10 h-40" />

  - 建立穩定且能安全變更的工作流程
  - 限制半成品數量
  - 移除非必要需求
  - 縮小開發規模與開發時間來持續產出

---
layout: center
---

# 第二步 - 放大回饋

<img src="/20250327/second-way1.png.webp" class="m-10 h-40" />

  - 將交付後的回饋有效的帶回開發源頭
    - 再次從源頭控管品質
    - 持續跟進業務目標，排除非必要需求
  - 設法減少、根除計劃外工作
  - 改進日常重複性工作

---
layout: center
---

# 第三步 - 持續學習與實驗的文化

<div class="flex items-center justify-center h-full gap-4 mt-4">

  <img src="/20250327/third-way.png.webp" class="m-10 h-40" />

</div>

  - 創造高度信任、樂於嘗試、分享的風氣
  - 減少低度信任、盲從與恐懼的風氣
  - 增加時間用於 IT 內部需求開發、改進
  - 形成更有韌性的開發流程


---
layout: image-right

# the image source
image: /20250327/the-unicorn-project.png
backgroundSize: 50%

---

# 獨角獸專案
- 鳳凰專案姊妹作
- 風凰專案同一時間，開發部工程師的故事...

---
layout: center
---

# 題外話
- 有興趣入手的話，這系列不要買電子書
- 固定版面 epub，排版不能放大縮小
- 字小眼睛痛

<div class="flex items-center justify-center h-full gap-4 mt-4">

  <img src="/20250327/screenshot_2025_03_26T01_41_23+0800.png" class="h-80" />
  <img src="/20250327/screenshot_2025_03_26T01_41_31+0800.png" class="h-80" />

</div>

---
layout: center
---

<div class="flex flex-col items-center justify-center h-full">
  <h1>The end</h1>
  <PoweredBySlidev />
</div>

