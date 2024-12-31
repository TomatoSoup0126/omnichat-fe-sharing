---
background: https://images.unsplash.com/photo-1582310849090-1619cba6c7c4
class: text-center
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
title: FE Sharing 2024.09.26
mdc: true
monaco: true
---

# Sharing 2025.01.02

Jonathan

---
layout: center
---

# 機器人 2.0 生存指南

- <Link to="unfortunately-involved-in-making-new-cards" title="不幸被捲入要做新卡片的話"/>
- <Link to="architecture" title="架構"/>
- <Link to="interesting-places" title="有趣的地方"/>
- <Link to="derivative" title="衍生物"/>

---
routeAlias: unfortunately-involved-in-making-new-cards
layout: center
---

# 不幸被捲入要做新卡片的話
- <Link to="making-new-preview-card" title="做預覽卡片"/>
- <Link to="writing-zod-schema" title="寫 zod schema"/>
- <Link to="making-new-edit-card" title="做編輯卡片"/>
- <Link to="calculating-edge" title="計算 edge (選擇性)"/>

---
routeAlias: making-new-preview-card
---

## 做預覽卡片 - 1

新增預覽卡片元件到
```
src/views/BotBuilder/components/BotBuilder/PreviewCards/NewPreviewCard.vue
```

```vue
<script setup lang="ts">
const props = defineProps<{
  data: any
}>();
</script>

<template>
  <div>
    <h1>init preview card</h1>
  </div>
</template>
```



---
layout: two-cols-header
---

## 做預覽卡片 - 2

將元件引入預覽入口
```
/src/views/BotBuilder/components/BotBuilder/Modules/MessageCard.vue
```

::left::

```vue
<script setup lang="ts">
import ImageCarouselPreview from '...';
import InvoicePreviewCard from '...';

const typeMap = computed(() => ({
  // ...
  [MESSAGE_TYPE.CHATBOT_IMAGE_CAROUSEL]: {
    component: ImageCarouselPreview,
    badgeText: '圖片輪播訊息'
  },
  [MESSAGE_TYPE.INVOICE]: {
    component: InvoicePreviewCard,
    badgeText: '發票模組'
  }
}));
</script>
```

::right::

<div class="w-50 h-10">
  <img src="/20250102/截圖 2024-12-31 下午3.11.34.png">
</div>

---

## 做預覽卡片 - 3

打開 [Figma](https://www.figma.com/design/lt75ZB1grmookZ2PpooMtd/Web-UI-%E9%80%B2%E9%9A%8E%E8%87%AA%E5%8B%95%E5%8C%96%E5%8A%9F%E8%83%BD(%E4%B8%80)?node-id=6371-143476&t=dOZ5fzPmiaK5k3Wh-4) 看設計稿

看看要實作的預覽有沒有其他長很像的

若有，應該會有些拆分出來的元件可以用

| ChatBubble | ImagePlaceHolder | RoundContainer | DashBorderBubble |
|:---:|:---:|:---:|:---:|
| <img src="/20250102/截圖 2024-12-31 下午3.31.46.png" width="120" class="mx-auto"> | <img src="/20250102/截圖 2024-12-31 下午3.33.31.png" width="120" class="mx-auto"> | <img src="/20250102/截圖 2024-12-31 下午3.34.27.png" width="120" class="mx-auto"> | <img src="/20250102/截圖 2024-12-31 下午3.36.03.png" width="120" class="mx-auto"> |

---
routeAlias: writing-zod-schema
---

## 寫 zod schema - 1

新增對應的新卡片 schema
```
src/views/BotBuilder/composable/useMessageBlockSchemaValidation/NewPreviewCardSchema.ts
```

```ts
import { z } from 'zod';
import { MESSAGE_TYPE } from '@/constants/messageType';
import { baseMessageSchema } from '@/views/BotBuilder/composable/useMessageBlockSchemaValidation/shareSchema';

export const newPreviewCardSchema = baseMessageSchema.extend({
  type: z.literal(MESSAGE_TYPE.NEW_PREVIEW_CARD),
  // ...
});

// 寫好的 schema 可以直接匯出成 type
export type NewPreviewCardType = z.infer<typeof newPreviewCardSchema>;
```

<div class="w-80">
  <img src="/20250102/截圖 2025-01-01 凌晨12.42.03.png" class="w-full">
</div>

---

chatbotCrossChannelConnectSchema

```ts
import { z } from 'zod';
import { MESSAGE_TYPE } from '@/constants/messageType';
import { baseMessageSchema } from '@/views/BotBuilder/composable/useMessageBlockSchemaValidation/shareSchema';

import i18n from '@/plugins/i18n';

const { t } = i18n.global;

export const crossChannelConnectSchema = z.object({
  crossChannelConnectId: z.string({
    message: t('THIS_FIELD_IS_REQUIRED')
  }).min(1, {
    message: t('THIS_FIELD_IS_REQUIRED')
  })
});

export const chatbotCrossChannelConnectSchema = baseMessageSchema.extend({
  type: z.literal(MESSAGE_TYPE.CROSS_CHANNEL_CONNECT),
  extension: crossChannelConnectSchema
});
```

---

jsonApiSchema

```ts {maxHeight:'300px'}
const literalSchema = z.union([z.string(), z.number(), z.boolean(), z.null()]);
const jsonSchema: z.ZodSchema<Json> = z.lazy(() => z.union([literalSchema, z.array(jsonSchema), z.record(jsonSchema)]));
export const jsonApiExtensionSchema = z.object({
  // ...
  requestBody: z.union([
    z.literal(''),
    z.string().refine(
      (value) => {
        try {
          const parsed = JSON.parse(value);
          return jsonSchema.safeParse(parsed).success;
        } catch {
          return false;
        }
      },
      { message: 'Invalid JSON string' }
    )
  ]).optional()
});

export const jsonApiSchema = baseMessageSchema.extend({
  type: z.literal(MESSAGE_TYPE.JSON_API),
  extension: jsonApiExtensionSchema
});

```

---

## 寫 zod schema - 2

放到 useMessageBlockSchemaValidation 中處理全域驗證

```ts
import { newPreviewCardSchema } from '...';

export const messageSchema = z.discriminatedUnion('type', [
  // ...
  newPreviewCardSchema,
]);
```

---
routeAlias: making-new-edit-card
layout: two-cols-header
---

## 做編輯卡片 - 1

新增編輯卡片元件到
```
src/views/BotBuilder/components/BotBuilder/Cards/NewEditCard.vue
```

::left::

```vue
<script setup lang="ts">
import Collapsible from '@/views/BotBuilder/components/Collapsible.vue';
const extension = defineModel('extension');
</script>

<template>
  <Collapsible :error-message="errorMessage">
    <template #subtitle>
      <h1>collapse title</h1>
    </template>
    {{ extension }}
  </Collapsible>
</template>
```

::right::

<div class="w-60">
  <img src="/20250102/截圖 2024-12-31 晚上11.10.45.png">
</div>

---

## 做編輯卡片 - 2

將剛剛的編輯卡片元件引入到
```
src/views/BotBuilder/components/BotBuilder/Cards/index.ts
```

```ts
export { default as NewEditCard } from './NewEditCard.vue';

export const cardList: T_CardTypes[] = [
  // ...
  'FlowCard',
  'CrossChannelConnectCard',
  'NewEditCard',
];

export const cardMap = {
  // ...
  NewEditCard: {
    icon: 'icon_name',
    name: 'Display name',
    type: 999,
    extension: {
      // init extension
    }
  }
}
```

---

## 做編輯卡片 - 3

新增卡片 type 到
```
src/views/BotBuilder/types.ts
```

```ts
export const MapCardTypes: Record<number, T_CardTypes> = {
  // ...
  999: 'NewEditCard',
};
```

---
routeAlias: calculating-edge
---

## 計算 edge - 1

如果新卡片會往其他模組連線的話，視結構而定可能需要計算 edge

新增計算 edge 的 function
```
/src/views/BotBuilder/composable/useBotsEdges.ts
```

```ts
export const useBotsEdges = () => {
  // ...
  const buttons = computed<Buttons>(() => messageBlocks.value.map((messageBlock: any) => {
    // ...
    switch (extension.type) {
      case 501:
          return generateUserInputButtonList({
            messageBlockId,
            extension
          });
      case 999:
        return generateNewCardButtonList() // return Button[];
    }
  }));
};
```

---
layout: two-cols-header
---

## 計算 edge - 2

edge 主要包含
- 從哪個模組內出發 (edge source)
- 從哪個 handle 出發 (edge source handle)
- 到哪個模組 (edge target)

::left::
```ts
interface Button {
  messageBlockId: string // 出發模組 id
  button_id: string      // 出發按鈕 id
  uuid: string           // 出發按鈕所連接的模組 id
  labelConfig?: {
    label: string
    style: {
      bgColor: string
    }
  }
}
```

::right::

<div class="w-full h-full flex justify-center items-center flex-col gap-4">
  <img src="/20250102/截圖 2025-01-01 凌晨12.28.38.png" class="w-full">
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

BotBuilder.vue
```vue {all|3|4-8|9-17}
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

<div>
  <ul>
    <v-click at="1">
      <li>
        BuilderNavbar
        <ul>
          <li>全模組資料驗證</li>
          <li>儲存</li>
          <li>路由跳轉</li>
        </ul>
      </li>
    </v-click>
    <v-click at="2">
      <li>
        BotFlow
        <ul>
          <li>接收模組資料進行渲染</li>
          <li>編輯當下選擇的模組ID</li>
          <li>編輯所有模組的座標</li>
        </ul>
      </li>
    </v-click>
    <v-click at="3">
      <li>
        SettingPanel
        <ul>
          <li>編輯當下選擇的模組</li>
          <li>控制 BotFlow 的縮放行為</li>
        </ul>
      </li>
    </v-click>
  </ul>
</div>

---
routeAlias: interesting-places
layout: center
---

# 有趣的地方
- 模組資料轉換成節點 (nodes)
- 計算模組間的連線 (edges)
- 按鈕與模組的連接點 (handle)
- 找出異動的模組
- 模組資料驗證

---
layout: image-right

# the image source
image: /20240927/截圖 2024-09-26 中午12.21.04.png
backgroundSize: contain

---

## 模組資料轉換成節點 (node)

每個模組在 VueFlow 中被視為一個 node

node example:

```json
{
  // 模組本身的 id
  "id":"ce3b9128-ec0f-410d-a66f-ad5c513e3959",
  // 在 VueFlow 中的座標
  "position":{
    "x": -117,
    "y": 345
  },
  // 模組本身的資料
  "data": { ... },
  // 辨識模組為機器人自身模組或外部模組
  "type": "default",
  // 供後續連線用的設定
  "sourcePosition": "left",
  "targetPosition":"right"
}
```

---

## 模組資料轉換成節點 (node)

useBotsNodes.ts

```ts {*}{maxHeight:'350px'}
export const useBotsNodes = ({ messageBlocks, positionsData }: { messageBlocks: any, positionsData: any }) => {
  const getNodePosition = (messageBlockId: string) => positionsData.value?.[messageBlockId] || {
    x: 0,
    y: 0
  };

  const wrapToFlowNode = (messageBlock: MessageBlock) => ({
    id: messageBlock.id,
    position: getNodePosition(messageBlock.id),
    data: messageBlock,
    type: messageBlock?.isExternal ? 'external' : 'default',
    sourcePosition: 'left',
    targetPosition: 'right'
  });

  const botsNodes = computed(() => messageBlocks.value
    .map((messageBlock: MessageBlock) => wrapToFlowNode(messageBlock))
  );

  return {
    botsNodes
  };
};

```

---

## 模組資料轉換成節點 (node)

BotFlow.vue
```vue {*}{maxHeight:'400px'}
<script setup lang="ts">
import { useBotsNodes } from '@/views/BotBuilder/composable/useBotsNodes';

const { botsNodes } = useBotsNodes({
  messageBlocks: computedMessageBlock,
  positionsData: botPositionsData
});
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
layout: image-right

# the image source
image: /20240927/截圖 2024-09-26 上午11.19.32.png
backgroundSize: contain

---

## 計算模組間的連線 (edge)

基於模組內的按鈕建立成 edge 的資料格式
(buttons, replies, carousel, nextblockUUID...)

edge example:

```json
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

基於模組內的按鈕建立成 edge 的資料格式 (buttons, replies, carousel, nextblockUUID...)

原始資料長這樣 

```json {*}{maxHeight:'300px'}
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
  });

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
layout: image-right

# the image source
image: /20240927/截圖 2024-09-24 下午4.16.19.png
backgroundSize: contain

---

## 按鈕與模組的連接點 (handle)

<ul class="mt-6">
  <li>edge 方向左進右出</li>
  <li>模組左上方進 (target)</li>
  <li>模組內部各個按鈕出 (source)</li>
</ul>

---
layout: two-cols-header
---

## 按鈕與模組的連接點 (handle)

::left::

NodeHandleWrapper.vue
```vue
<script setup lang="ts">
import { Handle, Position } from '@vue-flow/core';
</script>

<template>
  <div class="node-handle-wrapper">
    <Handle
      type="target"
      :position="Position.Left"
      :connectable="false"
      class="node-handle"
    />
    <slot />
  </div>
</template>
```

::right::

NestHandleWrapper.vue
```vue {*}{maxHeight:'400px'}
<script setup lang="ts">
import { Handle, Position } from '@vue-flow/core';

const position = computed(() => {
  switch (props.position) {
    case 'bottom':
      return Position.Bottom;
    default:
      return Position.Right;
  }
});
</script>

<template>
  <div class="nest-handle-wrapper">
    <Handle
      :id="props.handleId"
      type="source"
      :position="position"
      class="nest-handle"
      :class="[
        {
          'nest-handle--disconnect': !isConnected,
          'nest-handle--connect': isConnected,
          'nest-handle--relative': isRelativeStyle,
          'opacity-0': !props.showHandle,
        },
        `nest-handle--${props.position}`,
      ]"
      :connectable="false"
    />
    <slot />
  </div>
</template>
```

---
layout: image-right

# the image source
image: /20240927/截圖 2024-09-26 上午11.37.31.png
backgroundSize: contain

---

## 模組的連接點

DefaultModuleCard.vue
```vue
<template>
  <NodeHandleWrapper>
    <div
      class="default-module-card-container"
      :class="{ 'default-module-card-container--selected': props.selected }"
    >
      ...
    </div>
  </NodeHandleWrapper>
</template>
```

---
layout: image-right

# the image source
image: /20240927/截圖 2024-09-26 上午11.37.39.png
backgroundSize: contain

---

## 按鈕的連接點

DefaultModuleCard.vue
```vue
<template>
  <NestHandleWrapper
    :key="button.button_id"
    :handle-id="button.button_id"
    :show-handle="!isLinkButton"
  >
    <div class="relative">
      ...
    </div>
  </NestHandleWrapper>
</template>
```

---

## 找出異動的模組
新版 save api 只儲存異動的模組

異動可以是模組修改位置、修改內容、刪除

payload example:
```json {*}{maxHeight:'350px'}
{
  "upsertList": [
    {
      "blockId": "64705cfb-0a44-4d6b-b2fc-d24d77392a0d",
      "blockName": "真人客服",
      "title": "Demo",
      "messages": [
        {
          "id": "a9a27411-167e-410e-a0a5-8b33c752a0c4",
          "title": "文字訊息",
          "message": "幫你找真人客服您",
          "type": 301,
          "expand": true,
          "isAskForHuman": true,
          "extension": {
            "title": "幫你找真人客服您",
            "buttons": []
          }
        }
      ],
      "isNew": false,
      "position": {
        "x": 934.4053235494302,
        "y": 153.59115199695935
      }
    }
  ],
  "deleteIdList": [
    "3cea30c7-3b23-45be-a045-08a19cfa15d1"
  ]
}
```

---

## 找出異動的模組

useDifferentCollection.ts
```ts {*}{maxHeight:'400px'}
import { isEqual, differenceBy, filter, omit } from 'lodash-es';
import type { MessageBlock, PositionsData } from '@/views/BotBuilder/types';

export const useDifferentCollection = (
  {
    originalBotMessageBlocksData,
    botMessageBlocksData,
    originalBotPositionsData,
    botPositionsData
  }
) => {
  const differentMessageBlockCollection = computed(() => {
    const originalDataList = originalBotMessageBlocksData.value;
    const editedDataList = botMessageBlocksData.value;
    // 找出已被刪除的模組 id
    const deletedIdCollection = differenceBy(originalDataList, editedDataList, 'id').map((data) => data.id);
    // 找出新增或已被修改的模組
    const editedCollection = filter(editedDataList, (editData) => {
      const originalData = originalDataList.find((data) => data.id === editData.id);
      return !originalData || !isEqual(originalData, editData);
    });
    const editedIdCollection = editedCollection.map((data) => data.id);
  
    return {
      editedCollection,
      editedIdCollection,
      deletedIdCollection
    };
  });

  const differentPositionsCollection = computed(() => {
    const originalDataList = originalBotPositionsData.value;
    const editedDataList = botPositionsData.value;

    const editedCollection: PositionsData = {};
    // 從 editedDataList 中找出新增或已被修改的模組座標
    Object.keys(editedDataList).forEach((key) => {
      const isExistInOriginal = originalDataList[key];
      if (!isExistInOriginal || !isEqual(originalDataList[key], editedDataList[key])) {
        editedCollection[key] = editedDataList[key];
      }
    });

    const editedIdCollection = Object.keys(editedCollection);

    return {
      editedCollection,
      editedIdCollection
    };
  });

  const totalDifferentCount = computed(() => {
    const {
      editedIdCollection: editedMessageBlockIdCollection,
      deletedIdCollection: deletedMessageBlockIdCollection
    } = differentMessageBlockCollection.value;
    const {
      editedIdCollection: editedPositionIdCollection
    } = differentPositionsCollection.value;

    // 用 Set 整理出所有異動的異動數量
    return new Set([
      ...editedMessageBlockIdCollection,
      ...deletedMessageBlockIdCollection,
      ...editedPositionIdCollection
    ]).size;
  });

  const differentEditIdCollection = computed(() => {
    const {
      editedIdCollection: editedMessageBlockIdCollection
    } = differentMessageBlockCollection.value;
    const {
      editedIdCollection: editedPositionIdCollection
    } = differentPositionsCollection.value;
    // 用 Set 整理出所有異動的 id
    return new Set([
      ...editedMessageBlockIdCollection,
      ...editedPositionIdCollection
    ]);
  });

  const generateUpsertFormat = (messageBlock: MessageBlock): UpsertItem => ({
    blockId: messageBlock.id,
    blockName: messageBlock.name,
    title: messageBlock.title,
    messages: messageBlock.messages?.map((message) => omit(message, ['grabbingIndex', 'idx', 'isGrabbing', 'index'])),
    isNew: !originalBotMessageBlocksData.value.find((originalData) => originalData.id === messageBlock.id) && messageBlock.type !== 'external',
    ...(messageBlock.isAskForHuman && { isAskForHuman: messageBlock.isAskForHuman })
  });

  const upsertList = computed(() => {
    const editList: UpsertItem[] = [];

    differentEditIdCollection.value.forEach((id) => {
      const matchedMessageBlock = botMessageBlocksData.value.find((messageBlock) => messageBlock.id === id);
      const matchedPosition = botPositionsData.value[id];
      if (matchedMessageBlock) {
        editList.push({
          ...generateUpsertFormat(matchedMessageBlock),
          position: matchedPosition
        });
      } else {
        editList.push({
          blockId: id,
          position: matchedPosition
        });
      }
    });

    return editList;
  });

  return {
    editedPositionsCollection: computed(() => differentPositionsCollection.value.editedCollection),
    editedMessageBlockCollection: computed(() => differentMessageBlockCollection.value.editedCollection),
    deletedMessageBlockIdCollection: computed(() => differentMessageBlockCollection.value.deletedIdCollection),
    totalDifferentCount,
    upsertList
  };
};

```

---

## 模組資料驗證

使用 zod 做 schema 即時驗證

useMessageBlockSchemaValidation.ts
```ts {*}{maxHeight:'350px'}
import { z } from 'zod';

export const messageSchema = z.discriminatedUnion('type', [
  chatbotTextButtonSchema,
  chatbotTextSchema,
  chatbotPhotoSchema,
  chatbotCarouselSchema,
  chatbotCarouselSquareSchema,
  chatbotUserInputSchema,
  jsonApiSchema,
  chatbotQuickReplySchema,
  chatbotConditionSchema
]);

const messageBlockSchemaValidation = (messageBlock: MessageBlock) => {
  const schema = z.object({
    id: z.string().min(1),
    name: z.string().min(1),
    messages: z.array(messageSchema).nonempty().refine(validateLastMessagePosition)
  });

  const result = schema.safeParse(messageBlock);
  return result.success;
};

export const useMessageBlocksSchemaValidation = (messageBlocks: ComputedRef<MessageBlock[]>) => {
  const validateList = computed(() => messageBlocks.value
    .filter(({ type }) => type !== 'external')
    .map((messageBlock) => messageBlockSchemaValidation(messageBlock)));
  const isAllValidate = computed(() => validateList.value.every((validate) => validate));
  return isAllValidate;
};

```

---

## 模組資料驗證

使用 zod 做 schema 即時驗證

chatbotConditionSchema.ts
```ts {*}{maxHeight:'350px'}
import { z } from 'zod';
import { MESSAGE_TYPE } from '@/constants/messageType';
import { baseMessageSchema } from '@/views/BotBuilder/composable/useMessageBlockSchemaValidation/shareSchema';

const baseConditionFilterSchema = z.object({
  operator: z.string().min(1),
  values: z.array(z.union([z.string(), z.number(), z.boolean()])).or(z.null())
});

const attributeConditionFilterSchema = baseConditionFilterSchema.extend({
  attributeType: z.number(),
  attributeKey: z.string().min(1)
});

const tagConditionFilterSchema = baseConditionFilterSchema.extend({
  filterType: z.literal('tag'),
  values: z.array(z.union([z.string(), z.number(), z.boolean()])).nonempty()
});

const systemAttributeConditionFilterSchema = attributeConditionFilterSchema.extend({
  filterType: z.literal('systemAttribute'),
  operator: z.string().min(1),
});

const customAttributeConditionFilterSchema = attributeConditionFilterSchema.extend({
  filterType: z.literal('customAttribute'),
  operator: z.string().min(1),
  values: z.union([
    z.array(z.union([z.string().min(1), z.number(), z.boolean()])).nonempty(),
    z.null()
  ])
});

const conditionFilterSchema = z.discriminatedUnion('filterType', [
  tagConditionFilterSchema,
  systemAttributeConditionFilterSchema,
  customAttributeConditionFilterSchema
]);

export const chatbotConditionSchema = baseMessageSchema.extend({
  type: z.literal(MESSAGE_TYPE.CONDITION),
  extension: z.object({
    filters: z.array(conditionFilterSchema).nonempty()
  })
});

```

---
routeAlias: derivative
layout: center
---

# 衍生物

- usePlaceholder

---
layout: image-right

# the image source
image: /20240927/截圖 2024-09-25 下午5.23.08.png
backgroundSize: contain
---

## usePlaceholder

<ul class="mt-6">
  <li>卡片預覽有各種 placeholder 的樣式</li>
  <li>每種卡片 placeholder 文字內容都不同</li>
</ul>

---

## usePlaceholder

```vue {*}{maxHeight:'400px'}
<script setup lang="ts">
import { usePlaceholder } from '@/views/BotBuilder/composable/usePlaceholder';

const {
  showPlaceholder,
  displayString
} = usePlaceholder({
  computedRef: computed(() => props.button?.title),
  placeholderString: t('BOT_BUILDER.KEY88')
});
</script>

<template>
  <div
    class="card-button"
    :class="{ '!text-primary-003': showPlaceholder }"
  >
    {{ displayString }}
  </div>
</template>
```

---
layout: center
---

<div class="flex flex-col items-center justify-center h-full">
  <h1>The end</h1>
  <PoweredBySlidev />
</div>

