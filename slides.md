---
background: https://images.unsplash.com/photo-1623973792500-d2fa0aad9723
class: text-center
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
title: FE Sharing 2025.11.27
mdc: true
monaco: true
---

# Sharing 2025.11.27

Jonathan

---
layout: image-right

# the image source
image: /20250327/the-unicorn-project.png
backgroundSize: 50%

---

# 獨角獸專案
- 出版於 2019
- 鳳凰專案相隔六年的姊妹作
- 從 **開發者** 視角看軟體開發與維運
- 同一個時間軸，不同的故事

---
layout: image
image: /20251127/timeline.png
backgroundSize: 70%
---

---
layout: center
--- 

# 故事是這樣的

---
layout: image-right
image: /20251127/maxine.webp
backgroundSize: 50%
--- 

# 主角 Maxine
- 公司裡的資深開發人員 / 架構師
- 開源專案作者，熟悉開源軟體開發流程
- 因為一次嚴重的薪資核算系統故障而被咎責（非他的鍋）
- 被調職到鳳凰專案團隊，作為懲罰

---
layout: center
--- 

# 鳳凰專案的慘況
### Maxine 報到後發現...
- 申請開發環境、取得專案需要數 n 張工單，流程走一半還會被關單
- 兩個禮拜還無法啟動 local 環境
- 程式碼耦合性極高，改一行壞全身
- Build 專案就需要數小時，而且難以成功
- 測試環境與生產環境不一致，經測試過的版本上線後絕對會有問題

---
layout: image-right
image: /20251127/Redstarbird.webp
backgroundSize: 50%
--- 

# 反抗軍
- 一群想改變現狀的工程師
- 試圖繞過官僚體制
- 建立自動化建置、重構架構
- 目標：讓開發變得更容易、更快速

---
layout: center
---

# 埃瑞克提出的五大理念
- <Link to="ideal-1" title="局部性與簡單性"/>
- <Link to="ideal-2" title="專注、流暢與快樂"/>
- <Link to="ideal-3" title="日常工作的改善"/>
- <Link to="ideal-4" title="心理安全感"/>
- <Link to="ideal-5" title="以客戶為中心"/>

---
routeAlias: ideal-1
layout: center
---

# 區域性和簡潔性
## Locality and Simplicity

---
layout: center
---

<v-clicks>

- 複雜度是開發者的敵人
- 要達成區域性和簡潔性，解耦是關鍵
- 好的架構應該讓團隊能獨立運作
- 不需要為了改一個小功能而與其他十個團隊協調

</v-clicks>
<!-- 程式碼、流程、組織中都適用，也可以衍生成康威定律 -->

---
routeAlias: ideal-2
layout: center
---

# 專注、流暢與快樂
## Focus, Flow, and Joy

---
layout: center
---

<v-clicks>

- 開發者在「心流」(Flow) 狀態下效率最高
- 等待（建置、工單、審核）會打斷心流
- 目標是減少等待時間，讓開發者專注於解決問題
- 開發應該是快樂的，而不是充滿挫折

</v-clicks>

---
routeAlias: ideal-3
layout: center
---

# 持續改善日常工作
## Improvement of Daily Work

---
layout: image-right
image: /20251127/Andon_Otomasyon_Panosu2.jpg
backgroundSize: 50%
---

# 豐田安燈系統

- 賦予生產線的員工權力，發現異常可以立即停下生產線
- 每次停下生產線時，都需要進行記錄、找出問題並解決

---
layout: center
---

<v-clicks>

- 不要只顧著趕新功能
- 必須持續償還技術債
- 改善開發流程本身就是工作的一部分
- 兩者都會受到時間複利的影響

</v-clicks>
<!-- 技術債和改善開發流程的複利效益 -->

---
routeAlias: ideal-4
layout: center
---

# 心理上的安全感
## Psychological Safety

---
layout: center
---

<v-clicks>

- 提升生產線效率的方法之一，就是讓員工有生理上的安全感
- 生理安全被滿足，有助於工作上的專注度與流暢度
- 進一步的則是心理上的安全感

</v-clicks>
---
layout: center
---

<v-clicks>

- 鼓勵誠實的溝通，而不是掩蓋問題
- 發生事故時，進行「不指責的事後檢討」
- 創新需要冒險，冒險需要安全感
- Google 研究指出，心理安全感是高效團隊的首要特徵

</v-clicks>

---
routeAlias: ideal-5
layout: center
---

# 以顧客為中心
## Customer Focus

---
layout: center
---

<v-clicks>

- 開發者應該知道自己在為誰開發
- 理解業務目標與客戶痛點
- 避免「功能工廠」模式
- 真正解決使用者的問題，而不只是交付程式碼

</v-clicks>

<!-- 如果你從來不知道你開發的東西有何回饋，你要如何創造有價值的東西？ 如果這功能不會讓客戶掏錢，那麼這個功能就是沒有價值的。 -->

---
layout: center
---

# 五大理念
- <Link to="ideal-1" title="局部性與簡單性"/>
- <Link to="ideal-2" title="專注、流暢與快樂"/>
- <Link to="ideal-3" title="日常工作的改善"/>
- <Link to="ideal-4" title="心理安全感"/>
- <Link to="ideal-5" title="以客戶為中心"/>


---
layout: center
---

### 當 Maxine 成功領導團隊取得初步成功
### 想在公司內部推行其他新專案時
### 遇到了阻礙...

---
layout: center
---

# 創新與維運的平衡

---
layout: center
---

# Geoffrey Moore 的框架

- <Link to="three-aspect-theories" title="三層面理論"/>
- <Link to="core-vs-context" title="核心業務與脈絡業務"/>

---
layout: center
---

# 三層面理論
## Three aspect theories

---
routeAlias: three-aspect-theories
layout: center
---

# 三層面理論
<v-clicks>

- **第一層 - 核心業務**
  - 成熟的產品或服務，是公司主要的獲利來源
  - 顧客、商業模式和營運模式都是已知、可預期的
  - 重點：效率、穩定性、最大化利潤

- **第二層 - 新興業務**
  - 快速成長的業務，未來可能成為第一層
  - 重點：推廣第一層、獲取市佔率

- **第三層 - 構思原型**
  - 創新實驗、早期的點子
  - 具有成為第二層的潛力
  - 重點：快速學習、快速驗證假設、抉擇是否存續
</v-clicks>
<!-- 隨著時間推移，H3 可能變成 H2，H2 變成 H1，H1 最終會衰退。 -->

---
layout: center
---

# 核心業務與脈絡業務
## Core vs Context

---
routeAlias: core-vs-context
layout: center
---

# 核心業務與脈絡業務

<v-clicks>

- **核心業務**
  - 企業的差異化優勢，客戶願意為此付錢的原因
  - 重點：**創新、差異化**
  
- **脈絡業務**
  - 為了支援核心業務運作而必須做的所有其他事情
  - 雖然必要，但客戶不會因為這些做得好而多付錢（但不做會扣分）
  - 重點：**標準化、自動化、外包** (目的是釋放資源給 Core)

</v-clicks>
<!-- 
所有的創新最終都會變成脈絡。
例如：電子郵件系統在 90 年代可能是核心優勢，現在只是脈絡（沒人會因為你的 Email 系統好而買你的產品，但如果 Email 壞了公司就停擺）。
-->

---
layout: center
---

# 鳳凰專案 vs 獨角獸專案
- **鳳凰專案**：從 IT 管理與維運 (Ops) 角度
  - 強調四種工作類型
  - 三步工作法
- **獨角獸專案**：從 開發者 (Dev) 角度
  - 強調五大理念
  - 三層面理論

---
layout: center
---

<div class="flex flex-col items-center justify-center h-full">
  <h1>The end</h1>
  <PoweredBySlidev />
</div>
