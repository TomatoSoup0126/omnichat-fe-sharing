---
background: https://images.unsplash.com/photo-1445711005973-54fe2a103826
class: text-center
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
title: FE Sharing 2024.09.27
mdc: true
monaco: true
---

# Sharing 2024.09.27

Jonathan

---
layout: center
---

# 機器人 2.0

- <Link to="key-points-of-refactor" title="改版重點、原則"/>
- <Link to="architecture" title="架構"/>
- <Link to="difficulties" title="難處"/>
- <Link to="derivative" title="衍生物"/>

---
routeAlias: key-points-of-refactor
layout: center
---

# 改版重點、原則 & ~~偷渡新功能~~
- 內部模組與按鈕的關聯性可視化
- 所有模組內容皆通過驗證才能儲存
- 儲存時只儲存異動模組
- 按鈕可連結到外部機器人模組
- 模組內部複製

---

## 機器人內部模組與按鈕的關聯性可視化

<div class="w-full h-full flex justify-center items-center flex-col gap-4">
  <img src="/20240927/截圖 2024-09-15 晚上9.01.13.png" class="w-full">
</div>

---


## 所有模組內容皆通過驗證才能儲存

<div class="w-full h-full flex justify-center items-center flex-col gap-4">
  <img src="/20240927/截圖 2024-09-23 下午3.58.18.png" class="w-full">
</div>

---

## 儲存時只儲存異動模組

<div class="w-full h-full flex justify-center items-center flex-col gap-4">
  <img src="/20240927/截圖 2024-09-23 下午4.02.13.png" class="w-full">
</div>

---

## 按鈕可連結到外部機器人模組

<div class="w-full h-full flex justify-center items-center flex-col gap-4">
  <img src="/20240927/截圖 2024-09-23 下午4.06.10.png" class="w-full">
</div>

---

## 模組內部複製

<div class="w-full h-full flex justify-center items-center flex-col gap-4">
  <img src="/20240927/截圖 2024-09-23 下午4.22.28.png" class="w-full">
</div>

---
routeAlias: architecture
layout: two-cols-header
---

# 架構

::left::

```vue
BotBuilder.vue
<template>
  <div>
    <BuilderNavbar @save="handleSave" />
    <BotFlow
      v-model:selectedNodeId="selectedNodeId"
      v-model:botPositionsData="botPositionsData"
      :bot-message-blocks-data="botMessageBlocksData"
    /> 
    <SettingPanel
      v-model:editingMessageBlock="botMessageBlocksData[selectedNodeIndex]"
      @zoom-in="handleZoomIn"
      @zoom-out="handleZoomOut"
      @zoom-to-fit="handleZoomToFit"
      @zoom-to="handleZoomTo"
      @edit-to="handleEditTo"
      @auto-layout-graph="autoLayoutGraph"
    />  
    </div>
  </div>
</template>

```

::right::

- BuilderNavbar
  - 全模組資料驗證
  - 儲存
  - 路由跳轉

- BotFlow
  - 接收模組資料進行渲染
  - 編輯當下選擇的模組ID
  - 編輯所有模組的座標

- SettingPanel
  - 編輯當下選擇的模組
  - 控制 BotFlow 的縮放行為

---
routeAlias: difficulties
layout: center
---

# 難處
- 計算模組間的連線 (edges)
- 模組內的按鈕與其他模組的連接點 (handle)
- 找出異動的模組
- 模組選擇器

---

## 計算模組間的連線 (edges)

要找出各模組內的每個互動按鈕(buttons, replies, carousel, nextblockUUID...)
並建立成 edge 的資料格式

edge example

```ts
{
  // 互動按鈕的所屬模組 id
  "source":"f1c716e1-dc38-44e1-8477-f29f266cb9e2",

  // 互動按鈕的 id
  "sourceHandle":"126dc19d-c49f-407a-962c-bf50d3a896dc",

  // 互動按鈕所連接的模組 id
  "target":"9fe190ed-ff72-411b-a71e-7215777cb7c5",

  // 部分 edge 的 label config
  "label":"滿足條件時觸發",
  "labelBgStyle": {
    "fill":"#2CD9C5"
  }
}

```
---

## 計算模組間的連線 (edges)

要找出各模組內的每個互動按鈕(buttons, replies, carousel, nextblockUUID...)
並建立成 edge 的資料格式

原始資料長這樣 

```ts {*}{maxHeight:'300px'}
{
  // 互動按鈕的所屬模組 id
  "id": "b9e37c8d-31ac-49a8-a7ef-932e610c5c9b",
  "chatbotId": "e5733dcb-be48-4582-aa66-68c0248af10b",
  "title": "Jonathan - flow",
  "name": "我有輪播訊息",
  "status": 1,
  "messages": [
    {
      "isGrabbing": false,
      "grabbingIndex": null,
      "id": "f8c5e9f9-e445-4efd-9312-7e1bee660462",
      "message": "Bot Message (機器人信息) Carousel",
      "extension": [
        {
          "imageUrl": "https%3A%2F%2Fs3-ap-southeast-1.amazonaws.com%2Fuat-caas-media-storage%2Fupload%2Fphotos%2Fuser-upload-photo%2F5c83f83e-4d89-4cac-b7a7-697cfefbe64a-2d9dfc3161a24bccada34d358ca7d14b.jpg",
          "title": "今天我要突破第二行，今天我要突破第二行，今天我要突破第二行，今天我要突破第二行，",
          "text": "我是輪播訊息一",
          "buttons": [
            {
              "isGrabbing": true,
              "grabbingIndex": 0,
              "title": "我會觸發歡迎模組",
              // 互動按鈕的 id
              "button_id": "dc782fc2-4c4c-4234-9d8e-5035656d5ef4",
              "type": "postback",
              // 互動按鈕所連接的模組 id
              "uuid": "9fe190ed-ff72-411b-a71e-7215777cb7c5",
              "idx": 0,
              "index": 1,
              "saveAttr": {
                "key": "GTG_GIFT_MOBILE",
                "value": "aaa",
                "type": 1
              }
            },
            {
              "title": "我也會觸發歡迎模組",
              // 互動按鈕的 id
              "button_id": "b10b0395-3298-4f29-a3a6-2f7fa5644ee5",
              "type": "postback",
              // 互動按鈕所連接的模組 id
              "uuid": "9fe190ed-ff72-411b-a71e-7215777cb7c5"
            },
            {
              "title": "帶我去 google",
              // 互動按鈕的 id
              "button_id": "aff17739-8940-4d94-9c42-d4f72f12d952",
              "type": "url",
              "url": "google.com"
            }
          ],
          "tempId": "0"
        },
        {
          "imageUrl": "https%3A%2F%2Fs3-ap-southeast-1.amazonaws.com%2Fuat-caas-media-storage%2Fupload%2Fphotos%2Fuser-upload-photo%2F5c83f83e-4d89-4cac-b7a7-697cfefbe64a-d2f4cca1b7be4835a4b93868de48f208.jpg",
          "title": "1600 * 900",
          "text": "我是輪播訊息二 我是輪播訊息二 我是輪播訊息二 我是輪播訊息二 我是輪播訊息二 我是輪播訊息二 我是輪播訊息二 我是輪播",
          "defaultAction": {
            "type": "uri",
            "title": "View detail",
            "url": "http://google.com"
          },
          "buttons": [
            {
              "title": "我會觸發真人客服",
              // 互動按鈕的 id
              "button_id": "243d7c74-8da5-42b5-a3f8-a2e275f570b7",
              "type": "postback",
               // 互動按鈕所連接的模組 id
              "uuid": "d4b75030-3664-4bee-865c-b8d114938794"
            }
          ],
          "tempId": "1"
        },
        {
          "imageUrl": "https%3A%2F%2Fs3-ap-southeast-1.amazonaws.com%2Fuat-caas-media-storage%2Fupload%2Fphotos%2Fuser-upload-photo%2F5c83f83e-4d89-4cac-b7a7-697cfefbe64a-b80d872ef4ab4da69cede9827956a2f2.jpg",
          "title": "600*400",
          "text": "我是副標題",
          "defaultAction": {
            "type": "uri",
            "title": "View detail",
            "url": "https://www.omnichat.ai/tw/"
          },
          "buttons": [
            {
              "title": "觸發真人客服",
              // 互動按鈕的 id
              "button_id": "1621402e-74d1-4f20-942b-9c87485133cf",
              "type": "postback",
              // 互動按鈕所連接的模組 id
              "uuid": "d4b75030-3664-4bee-865c-b8d114938794"
            }
          ],
          "tempId": "2"
        },
        {
          "imageUrl": "https%3A%2F%2Fs3-ap-southeast-1.amazonaws.com%2Fuat-caas-media-storage%2Fupload%2Fphotos%2Fuser-upload-photo%2F5c83f83e-4d89-4cac-b7a7-697cfefbe64a-fde1e779bf1d4970b8dd8a87fa8677c4.jpg",
          "title": "aaa",
          "buttons": [
            {
              "title": "aaa",
              // 互動按鈕的 id
              "button_id": "af3a38b7-6cbe-45ab-ad7a-cccb532d285b",
              "type": "url",
              "url": "http://asd.com"
            }
          ],
          "tempId": "3"
        }
      ],
      "imageAspectRatio": "square",
      "type": 203,
      "creationDate": "001721203345563",
      "modificationDate": "001721203345563",
      "status": 1,
      "idx": 0,
      "index": 1
    }
  ],
  "messageContent": null,
  "creationDate": "001718873734835",
  "modificationDate": "001726019980434",
  "isExternal": false
}

```
---

## 計算模組間的連線 (edges)

useBotsEdges.ts

```ts {*}{maxHeight:'400px'}
export const useBotsEdges = ({ messageBlocks }: { messageBlocks: any }) => {
  const buttons = computed<Buttons>(() => messageBlocks.value.map((messageBlock: any) => {
    const messageBlockId = messageBlock.id;
    const messages = messageBlock.messages ?? [];
    const extensions = Array.isArray(messages) ? messages.flatMap((message: any) => messageExtensionsWithMessageType(message)) : [];
    return extensions.flatMap((extension: Extension) => {
      switch (extension.type) {
        case 501:
          return generateUserInputButtonList({
            messageBlockId,
            extension
          });
        case 502:
          return generateJsonApiButtonList({
            buttonUUID: extension.messageId,
            messageBlockId,
            extension
          });
        case 504:
          return generateConditionalButtonList({
            buttonUUID: extension.messageId,
            messageBlockId,
            extension
          });
        default:
          const buttons = extension.buttons ?? extension.carousel ?? extension.replies ?? [];
          return buttons.map((button: any) => ({ ...button, messageBlockId }));
      }
    });
  }).flat());

  const wrapToFlowEdge = (button: Button) => ({
    id: _uuid(),
    source: button.messageBlockId,
    sourceHandle: button.button_id,
    target: button.uuid,
    type: 'default',
    ...button.labelConfig
      ? {
          label: button.labelConfig.label,
          labelBgStyle: { fill: button.labelConfig.style.bgColor }
        }
      : {}
  } as unknown as Edge);

  const botsEdges = computed(() => buttons.value.map((button) => wrapToFlowEdge(button)));

  return {
    botsEdges
  };
};

```

---

## 計算模組間的連線 (edges)

BotFlow.vue
```vue {*}{maxHeight:'400px'}
<script setup lang="ts">
import { useBotsEdges } from '@/views/BotBuilder/composable/useBotsEdges';

const props = defineProps<{
  botMessageBlocksData: MessageBlock[]
}>();

const { botsEdges } = useBotsEdges({ messageBlocks: computedMessageBlock });

</script>
<template>
  <div>
    <VueFlow
      class="bot-flow"
      :nodes="botsNodes"
      :edges="botsEdges"
      :max-zoom="10"
      :min-zoom="0.1"
      @nodes-initialized.once="handleNodesInitialized"
      @node-click="handleNodeClick"
      @pane-click="handleCloseDrawers"
      @node-drag-stop="handleNodeDrag"
      @selection-drag-stop="handleNodeDrag"
    >
  </div>
</template>
```

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
routeAlias: vue-devtool-next
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

# 差別
- 靜態檔案瀏覽
- 時間軸
- 模組關聯圖

---
routeAlias: extract-command
layout: center
---

# Vue extension - extract command

<div class="flex flex-col items-center justify-center h-full">
  <div
    style="background-image: url('/20240711/vue-official.png')"
    class="w-full h-30 bg-contain bg-no-repeat"
  />
</div>

---
layout: center
---

# ⌘ + P > Refactor
<div class="flex flex-col items-center justify-center h-full">
  <img class="mb-2 w-9/10" src="/20240711/截圖 2024-07-11 中午12.09.02.png" />
  <img class="w-9/10" src="/20240711/截圖 2024-07-11 中午12.05.58.png" />
</div>

---
layout: center
---

<div class="flex flex-col items-center justify-center h-full">
  <h1>The end</h1>
  <PoweredBySlidev />
</div>

