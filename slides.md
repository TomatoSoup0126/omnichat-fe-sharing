---
background: https://images.unsplash.com/photo-1514897575457-c4db467cf78e
class: text-center
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
title: FE Sharing 2024.07.18
mdc: true
monaco: true
---

# Sharing 2024.07.18

Jonathan

---
layout: center
---

# 今天的內容

- <Link to="mythical-man-month" title="人月神話"/>
- <Link to="file-previewer" title="Vue devtool next"/>

---
routeAlias: mythical-man-month
layout: image-left

# the image source
image: /20240711/cover-mythical-man-month.webp
backgroundSize: contain

---

# 人月神話
- 號稱軟體管理專案經典
- 1975 年上市
- 距今即將 50 年
- 雖然年代久遠，但有些東西拿到現在還是通用

---
layout: image-left

# the image source
image: /20240711/Fred_Brooks.jpg
backgroundSize: contain

---

# Frederick P. Brooks, Jr.
- IBM OS/360 軟體經理
- IBM 系統部主任
- 北卡羅來納大學電腦科學學系創系教授
- 1999 年圖靈獎得主

---
layout: center
---

# 部分章節

- <Link to="the-tar-pit" title="焦油坑"/>
- <Link to="the-mythical-man-month" title="人月神話"/>
- <Link to="aristocracy-democracy" title="專制、民主與系統設計"/>
- <Link to="the-second-system-effect" title="第二系統效應"/>
- <Link to="calling-the-shot" title="預估"/>
- <Link to="the-pounds-in-a-five-pound-sack" title="地盡其利，物盡其用"/>
- <Link to="plan-to-throw-one-away" title="失敗為成功之母"/>

---
routeAlias: the-tar-pit
layout: center
---

# 焦油坑 (The Tar Pit)

<img src="/20240711/La_Brea_Tar_Pits.jpg">
<p class="text-center">“ 對航海的人來說，擱淺的船就是燈塔 ”</p>

---
layout: center
---

<div class="w-full h-full flex justify-center items-center flex-col gap-4">
  <h2>軟體系統產品為何難開發？</h2>
  <img src="/20240711/截圖 2024-07-09 凌晨1.15.22.png" class="w-1/2">
</div>

---
layout: center
---

## 屬於工程師的喜樂與苦難

---
routeAlias: the-mythical-man-month
---

# 人月神話 (The Mythical Man-Month)

<div class="mt-8 flex gap-8 items-center">
  <div
    style="background-image: url('/20240711/f0026-01.jpg')"
    class="w-1/2 aspect-[3/4] bg-contain bg-no-repeat"
  />
  <div>
    <p class="text-center">“ 好菜都得多花些時間準備，為了讓你可以享受到更美味、更可口的佳餚，請您務必耐心稍待 ”</p>
  </div>
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <h1 class="text-center">軟體開發進度落後的主要原因</h1>
  <h1 class="text-center">缺乏合理的時間進度預估</h1>
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <h1 class="text-center">人月</h1>
  <h2 class="text-center">Man-Month</h2>
</div>

---

<div class="flex flex-col items-start justify-center h-full ml-60">
  <li class="text-center text-2xl list-none">衡量開發規模 / 成本的單位 （Ｏ）</li>
  <li class="text-center text-2xl list-none">衡量時程 / 產出的單位 （Ｘ）</li>
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <h1 class="text-center">「人力」和「工時」難以直接轉換</h1>
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <h2 class="text-center mb-4">可以轉換的前提：</h2>
  <h1 class="text-center">開發內容在不溝通的情況下能完全拆分、切割</h1>
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <div
    style="background-image: url('/20240711/截圖 2024-07-09 中午12.56.59.png')"
    class="w-1/2 aspect-[3/4] bg-contain bg-no-repeat"
  />
  <h3 class="text-center mt-3">完全無法切割的情況</h3>
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <div
    style="background-image: url('/20240711/截圖 2024-07-09 中午12.58.55.png')"
    class="w-1/2 aspect-[3/4] bg-contain bg-no-repeat"
  />
  <h3 class="text-center mt-3">理想中工時與人力完整互換的結果</h3>
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <div
    style="background-image: url('/20240711/截圖 2024-07-09 下午1.00.46.png')"
    class="w-1/2 aspect-[3/4] bg-contain bg-no-repeat"
  />
  <h3 class="text-center mt-3">需要溝通的可拆分任務</h3>
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <div
    style="background-image: url('/20240711/截圖 2024-07-09 下午1.02.24.png')"
    class="w-1/2 aspect-[3/4] bg-contain bg-no-repeat"
  />
  <h3 class="text-center mt-3">極度仰賴溝通的複雜任務</h3>
</div>

---

<div class="flex flex-col items-center justify-center h-full text-center gap-3">
  <h3>Brooks's law</h3>
  <h3>在一個已經進度落後的軟體項目上再增加人手</h3>
  <h3>只會使這個軟體項目進度更加落後</h3>
</div>

---

<div class="flex flex-col items-center justify-center h-full gap-3">
  <h3>作者的建議估算方法</h3>
  <div class="flex w-full gap-3 ml-60">
    <div class="w-2/5">
      <h3 class="mt-3">計畫 - 1 / 3</h3>
      <h3 class="mt-3">程式開發 - 1 / 6</h3>
      <h3 class="mt-3">元件測試、前期測試 - 1 / 4</h3>
      <h3 class="mt-3">整合測試、完成開發 - 1 / 4</h3>
    </div>
    <div class="w-3/5">
      <div class="border w-1/3 h-8 mt-3" />
      <div class="border w-1/6 h-8 mt-3" />
      <div class="border w-1/4 h-8 mt-3" />
      <div class="border w-1/4 h-8 mt-3" />
    </div>
  </div>
</div>

---
routeAlias: aristocracy-democracy
---

# 專制、民主與系統設計 (Aristocracy, Democracy, and System Design)

<div class="mt-8 flex gap-8 items-center">
  <div
    style="background-image: url('/20240711/dsc01711.jpeg')"
    class="w-1/2 aspect-[3/4] bg-contain bg-no-repeat"
  />
  <div class="-mt-[40px]">
    <p class="text-center">“ 蘭斯大教堂是個無與倫比的藝術結晶，一點也不會讓人有乏味和混亂的感覺... ”</p>
  </div>
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <h1 class="text-center">概念一致性</h1>
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <h1 class="text-center">概念應由少數人定義</h1>
  <h1 class="text-center">多數人實作時在框架/限制下發揮專長</h1>
</div>

---
routeAlias: the-second-system-effect
---

# 第二系統效應 (The Second-System Effect)

<div class="mt-8 flex gap-8 items-center">
  <div
    style="background-image: url('/20240711/f0066-01.jpg')"
    class="w-1/2 aspect-[1/1] bg-contain bg-no-repeat"
  />
  <div>
    <p class="text-center">“ 加一點點，加一點點，最後變成一大坨 ”</p>
  </div>
</div>

---

<div class="flex flex-col items-center justify-center h-full gap-3">
  <h1 class="text-center">開發者經手的第一項專案</h1>
  <h1 class="text-center">恰到好處，容易成功達成目標</h1>
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <h1 class="text-center">開發者經手的第二項專案</h1>
  <h1 class="text-center">容易過度設計</h1>
  <h1 class="text-center" v-click>之前想到但沒做、做過的酷東西都想加進來</h1>
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <h1 class="text-center">靠「自律」避免第二系統效應</h1>
  <ul>
    <li>尋求他人討論</li>
    <li>對誘惑的警覺</li>
    <li>針對加入的設計提出質疑</li>
    <li>確保設計、開發方向與原則、目標對齊</li>
  </ul>
</div>

---
routeAlias: calling-the-shot
---

# 預估 (Calling the Shot)

<div class="mt-8 flex gap-8 items-center">
  <div
    style="background-image: url('/20240711/29691b_med.jpeg')"
    class="w-1/2 aspect-[1/1] bg-cover bg-no-repeat bg-center"
  />
  <div>
    <p class="text-center ml-[30px]">“ 練習就是最好的教練 ”</p>
  </div>
</div>

---

<div class="flex flex-col items-center justify-center h-full gap-3">
  <h1 class="text-center">工程師開發時程的變因</h1>
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <div
    style="background-image: url('/20240711/截圖 2024-07-09 晚上10.37.12.png')"
    class="w-1/2 aspect-[3/4] bg-contain bg-no-repeat"
  />
  <h3 class="text-center mt-3">開發內容的複雜性會顯著影響人月</h3>
</div>

---

<div class="flex flex-col items-center justify-center h-full gap-3">
  <h2 class="text-center">不要拿短期產出效率做線性放大來預估時程</h2>
</div>

---

<div class="flex flex-col items-center justify-center h-full gap-3">
  <h2 class="text-center">Portman 電腦公司的數據顯示</h2>
  <h2 class="text-center">全職工程師一天只有 50% 時間能進行真正的程式開發</h2>
</div>

---

<div class="flex flex-col items-center justify-center h-full gap-3">
  <h2 class="text-center">IBM 的數據顯示</h2>
  <h2 class="text-center">溝通量低的團隊相較溝通量高的團隊</h2>
  <h2 class="text-center">產出可以到 6 倍以上</h2>
</div>

---
routeAlias: the-pounds-in-a-five-pound-sack
---

## 地盡其利，物盡其用 (The Pounds in a Five-Pound Sack)

<div class="flex flex-col items-center justify-center h-full">
  <div
    style="background-image: url('/20240711/Heywood_Hardy_-_Gods_Covenant_with_Noah_-_(MeisterDrucke-242233).jpg')"
    class="w-1/2 aspect-[5/3] bg-contain bg-no-repeat bg-center"
  />
  <p class="text-center">“ 創作者應該學學諾亞是怎麼將一大票東西塞進方舟的 ”</p>
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <h2 class="text-center mt-2">軟體開發的資源是有限的</h2>
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <h2 class="text-center mt-2">不應只求局部的最佳化</h2>
  <h2 class="text-center mt-2">應著重在呈現給客戶的效果</h2>
</div>

---
routeAlias: plan-to-throw-one-away
---

## 失敗為成功之母 (Plan to Throw One Away)

<div class="flex flex-col items-center justify-center h-full">
  <div
    style="background-image: url('/20240711/Collapse-Tacoma-Narrows-Bridge-Washington-state-1940.webp')"
    class="w-1/2 aspect-[5/3] bg-contain bg-no-repeat bg-center"
  />
  <p class="text-center">“ 這世界唯一不變的就是世界一直在變 ”</p>
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <h2 class="text-center mt-2">顧客的所有需求是無法一次滿足的</h2>
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <h2 class="text-center mt-2">習慣變化</h2>
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <h2 class="text-center mt-2">讓系統易於變化</h2>
  <ul class="mt-3">
    <li>模組化</li>
    <li>擴充性</li>
    <li>耦合性</li>
    <li>文件</li>
    <li>版控、版本追蹤</li>
  </ul>
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <h2 class="text-center mt-2">讓組織易於變化</h2>
  <ul class="mt-3">
    <li>成員、任務流動性</li>
    <li>重新配置</li>
    <li>資深管理、開發人員的換位培養</li>
  </ul>
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <h2 class="text-center mt-2">軟體維護</h2>
  <h3 class="text-center mt-2">修一個 bug 有 20~50% 的機率產生新的 bug</h3>
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <h2 class="text-center mb-2">軟體交付後的 bug 數量曲線</h2>
  <img src="/20240711/截圖 2024-07-10 晚上10.32.52.png" class="w-1/2">
</div>

---

<div class="flex flex-col items-center justify-center h-full">
  <h2 class="text-center mb-2">軟體開發 - 減少混亂的過程</h2>
  <h2 class="text-center mb-2">軟體維護 - 增加混亂的過程</h2>
</div>

---
layout: center
---

# Takeaways

- 人月神話：人力和工時難以互換，增加人力無法等價縮短工時
- Brooks's law：已落後進度的開發上再增加人手，只會讓它更加落後
- 概念完整性：開發者應維持系統的各種一致性
- 第二系統效應：開發者應靠自律來避免過度設計
- 時程預估：開發時程變因很多，切勿樂觀謹慎預估
- 變化：讓系統與組織利於變化

---
layout: iframe

# the web page source
url: https://devtools-next.vuejs.org/
---
layout: center
---

<div class="flex flex-col items-center justify-center h-full">
  <div
    style="background-image: url('/20240711/nuxt_devtool.png')"
    class="w-full h-full bg-contain bg-no-repeat"
  />
</div>

---
layout: center
---

# Use
- Chrome extension
- Vite plugin
- Standalone app

---
layout: center
---

<div class="flex flex-col items-center justify-center h-full">
  <h1>The end</h1>
  <PoweredBySlidev />
</div>

