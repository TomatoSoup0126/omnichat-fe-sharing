---
background: https://images.unsplash.com/photo-1606422296071-bbf764bfdb71?q=80&w=2950
class: text-center
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
title: FE Sharing 2025.06.26
mdc: true
monaco: true
---

# Sharing 2025.06.26

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

<v-click>

- CEO 告知他上面的兩位主管都請辭走人了

</v-click>

<v-click>

- 原地提拔成為 IT 部門主管

</v-click>

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
# Today's sharing
- <Link to="hermes-update" title="Hermes update"/>
- <Link to="edge-tooltip-menu" title="Edge tooltip & menu"/>
- <Link to="use-route-query" title="useRouteQuery"/>
- <Link to="slidev-extensions" title="Slidev extensions"/>

---
routeAlias: hermes-update
---

# Hermes update

- Container width toggle
- i18n support zh-tw & en

---
layout: iframe

# Hermes update
url: https://uat-pages.omnichat.ai/hermes?lang=zh-Hant#CustomerLimitReached
scale: 0.85
---
# Hermes site
---

# Use

```vue
<script setup>
import { t, locale } from '@/plugins/i18n';

const upgradeUrl = computed(() => {
  return locale.value === 'en' ? 'https://www.omnichat.ai/pricing' : 'https://www.omnichat.ai/tw/pricing';
});
</script>

<template>
  <LinkButton
    :text="t('trialExpired.upgrade')"
    :href="upgradeUrl"
  />
</template>
```

---
routeAlias: edge-tooltip-menu
layout: image-right
image: /public/20250626/Group 199983.png
backgroundSize: 80%
---

# Edge tooltip & menu

- Display a tooltip when hovering over an edge
- Display a menu when clicking on an edge

---
layout: image-right
image: /public/20250626/Group 19998.png
backgroundSize: 80%
---

# Tooltip / menu's position

- The tooltip / menu's position is relative to the cursor, not the edge.

---
layout: iframe

url: https://uat-console-v2.omnichat.ai/bot-builder/feffcc6c-9716-4f26-a28e-74c4f3378c8d?target=bea5016d-f962-4e3b-9a1f-856bfcdf805a
scale: 0.75
---
# Bot builder 2.0
---

# Implementation


#### BotFlow.vue
```vue {3-10}
<template>
  <div>
    <EdgeMenu
      :bots-edges="botsEdges"
      :bots-nodes="botsNodes"
    />
    <EdgeTooltip
      :bots-edges="botsEdges"
      :bots-nodes="botsNodes"
    />
    <VueFlow
      :nodes="botsNodes"
      :edges="botsEdges"
      ...
    />
  </div>
</template>
```

---

# Implementation

#### DefaultEdge.vue

```vue {2|4,13|6-11|15-20|22-30|all}{maxHeight:'400px'}
<script setup>
const { onEdgeMouseEnter, onEdgeMouseLeave, onEdgeClick } = useVueFlow();

onEdgeMouseEnter(({ edge, event }) => {
  if (edge.id === props.id) {
    isHovering.value = true;
    hoverEdgeId.value = edge.id;
    hoverEdgeMenuPosition.value = {
      x: event.clientX,
      y: event.clientY - 93
    };
  }
});

onEdgeMouseLeave(({ edge }) => {
  if (edge.id === props.id) {
    isHovering.value = false;
    hoverEdgeId.value = '';
  }
});

onEdgeClick(({ edge, event }) => {
  if (edge.id === props.id) {
    clickedEdgeId.value = edge.id;
    clickedEdgeMenuPosition.value = {
      x: event.clientX,
      y: event.clientY - 93
    };
  }
});
</script>
```
---
routeAlias: use-route-query
---

# useRouteQuery

#### asgard/src/components/composables/useRouteQuery.js
```js {all}
import { computed } from 'vue';
import { debounce } from 'lodash-es';

export function useRouteQuery() {
  const route = useRoute();
  const router = useRouter();

  const query = computed(() => route.query);
  const updateUrlQuery = debounce((value) => {
    router.replace({
      query: {
        ...query.value,
        ...value
      }
    });
  }, 400);

  return {
    query,
    updateUrlQuery
  };
}

```

---

# Use

```vue {all}
<script setup>
const { updateUrlQuery, query } = useRouteQuery();
const handleUpdateOption = (options) => {
  const updateQuery = {
    page: options.page,
    pageSize: options.itemsPerPage,
  };

  updateUrlQuery(updateQuery);
};
</script>

<template>
  <div>
    <span>{{ query }}</span>
    <DataTable
      :headers="headerList"
      :items="props.rowData"
      @update:options="handleUpdateOption"
    />
  </div>
</template>

```

---
routeAlias: slidev-extensions
layout: image-right
image: /public/20250626/slidev-extensions.png
backgroundSize: 80%
---

# Slidev extensions

- by Anthony Fu
- [marketplace](https://marketplace.cursorapi.com/items?itemName=antfu.slidev)

---
layout: image

image: /public/20250626/slidev-extensions-in-cursor.png
backgroundSize: 85%
---
---
layout: center
---

<div class="flex flex-col items-center justify-center h-full">
  <h1>The end</h1>
  <PoweredBySlidev />
</div>
