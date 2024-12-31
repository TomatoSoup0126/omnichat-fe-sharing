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
routeAlias: derivative
layout: center
---

# 衍生物

- RemoteSearchSelector
- useZodFormValidation 

---

## RemoteSearchSelector

開始有些長相和行為類似的 Selector 出現了

<div class="">
  <img src="/20250102/Group 19997.png" class="w-full">
</div>

---

## Use

```vue
<script setup lang="ts">
import RemoteSearchSelector from '@/components/RemoteSearchSelector/RemoteSearchSelector.vue';

const selectedId = defineModel<string>();

const imagemapRemoteSearchConfig = computed(() => ({
  fieldNames: {
    label: 'name',
    value: 'id',
  },
  filterOption,
  fetchListHandler: getCrossChannelConnectList,
  showAddItem: true,
  selectPlaceholder: t('BOT_BUILDER.KEY312'),
  addItemText: t('BOT_BUILDER.KEY314'),
  addItemLink: asgardCrossChannelConnectLink.value,
  noSearchResultText: t('BOT_BUILDER.KEY315'),
}));
</script>
<template>
  <RemoteSearchSelector
    ref="selector"
    v-model="selectedId"
    :config="imagemapRemoteSearchConfig"
  />
</template>
```

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

