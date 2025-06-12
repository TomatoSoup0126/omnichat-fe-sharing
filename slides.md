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