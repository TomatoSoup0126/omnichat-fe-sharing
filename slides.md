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

# 流程的改良

- 流程的目的是讓工作更順利，而不是增加困難
- 流程的設計要能夠符合使用者需求
- 使用者願意使用的流程，才有機會成功

---
layout: center
---

# 半成品危機 - 1

- 以工廠來說，原料進來到離廠之前，任何步驟後的產物都只算是半成品

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
  - 需求開始開發後，在交付給使用者之前，都是半成品
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

<v-click>

- 以軟體開發來說，便是需要有效的控管需求進入開發的數量

</v-click>

---
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

# 意識到手上的工作分類
- 了解每類工作占用時間的比例
- 明確意識到計劃外工作的存在
- 四種工作類型的資源分配比重

---
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
layout: center
---

# 開發人員忙碌程度 vs 需求等待時間
- 若所有需求的優先度都相同
- 等待時間 = 開發人員忙碌百分比 / 開發人員閒置百分比

<img src="/20250327/wait-time-vs-utilization.png" class="m-10 h-60" />


---
layout: center
---

# 三步工作法
- 建立可視性的工作流程，將開發規模縮小且短
- 設法根除計劃外工作
- 重複上述步驟，直到流程順暢

---
layout: center
---

<div class="flex flex-col items-center justify-center h-full">
  <h1>The end</h1>
  <PoweredBySlidev />
</div>

