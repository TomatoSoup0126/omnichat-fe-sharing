---
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
title: FE Sharing 2023.11.23
mdc: true
---

# Sharing 2023.11.23

<br>
<br>
<br>
<br>

Jonathan

---
transition: fade
---

# 按條件變動的 Header list ?

像醬的設計


<div  class="flex justify-center">
  <img src="/Group_20104.png" class="w-auto h-96">
</div>

---

# 按條件變動的 Header list ?

像醬的設計


<div  class="flex justify-center">
  <img src="/Group_20104_fill.png" class="w-auto h-96">
</div>

---

# 大家熟悉的 headerList 結構

```vue {all|2-9|16|all}
<script setup>
const headerList = [
  { text: 'Dessert (100g serving)', value: 'name',},
  { text: 'Calories', value: 'calories' },
  { text: 'Fat (g)', value: 'fat' },
  { text: 'Carbs (g)', value: 'carbs' },
  { text: 'Protein (g)', value: 'protein' },
  { text: 'Iron (%)', value: 'iron' }
]

const items = [...]
</script>

<template>
<v-data-table
  :headers="headerList"
  :items="items"
/>
</template>
```

---

# 舉個例子

<div style="height: 50vh; overflow-y: scroll;">

### 情境一
- conditionA ✅
- conditionB ✅

```js
const headerList = [
  defaultColumnA,
  defaultColumnB,
  columnA
  columnB,
  defaultColumnC
]
```
<br>

### 情境二
- conditionA ✅
- conditionB ❌

```js
const headerList = [
  defaultColumnA,
  defaultColumnB,
  columnA
  columnC,
  defaultColumnC
]
```
<br>

### 情境三
- conditionA ❌
- conditionB ✅

```js
const headerList = [
  defaultColumnA,
  defaultColumnB,
  columnB,
  defaultColumnC
]
```
<br>

### 情境四
- conditionA ❌
- conditionB ❌

```js
const headerList = [
  defaultColumnA,
  defaultColumnB,
  columnC,
  defaultColumnC
]
```

</div>

---
layout: two-cols-header
---

# 直接操作 headerList

::left::

```vue {all|2-6|8-14|14-23|all} {lines:true, startLine:1}
<script setup>
const headerList = reactive([
  defaultColumnsA,
  defaultColumnsB,
  defaultColumnsC,
])

watch(conditionA, () => {
  if (conditionA) {
    headerList.splice(2, 0, columnA)
  } else {
    // remove columnA from headerList
  }
})
watch(conditionB, () => {
  if (conditionB) {
    headerList.splice(array.length - 1, 0 , columnB)
    // remove columnC from headerList
  } else {
    headerList.splice(array.length - 1, 0 , columnC)
    // remove columnB from headerList
  }
})
</script>
```

::right::
<div v-after style="padding: 20px">

  ## Pros
  - 直觀(?)

  ## Cons
  - 條件變化下會需要不斷 splice, push, unshift 陣列
  - 需要注意陣列 index
  - 不好追蹤結果

</div>

---
layout: two-cols-header
---

# 用 computed 生成 headerList

::left::

```vue {all|2|3-7|8-10|12-16|18|20|all} {lines:true, startLine:1}
<script setup>
const headerList = computed(() => {
  const header = [
    defaultColumnA,
    defaultColumnB
  ]

  if (conditionA) {
    header.push(columnA)
  }

  if (conditionB) {
    header.push(columnB)
  } else {
    header.push(columnC)
  }

  header.push(defaultColumnC)

  return header
})
</script>
```

::right::
<div v-after style="padding: 20px">

  ## Pros
  - 由條件驅動
  - header 順序相對明顯
  - 每次都重新生成 header array，不用移除多餘的 column

  ## Cons
  - computed 運算時會重複對 array 進行空間操作
  - 部分預設 column 會在中途才加入 header array，不好看出哪些 column 是預設的

</div>

---
layout: two-cols-header
---

# computed 中使用展開 + 三元運算子

::left::

```vue {all|4,5,8|6|7|all} {lines:true, startLine:1}
<script setup>
const headerList = computed(() => {
  return [
    defaultColumnA,
    defaultColumnB,
    ...conditionA ? [columnA] : [],
    ...conditionB ? [columnB] : [columnC],
    defaultColumnC,
  ]
})
</script>
```

::right::
<div v-after style="padding: 0 20px;">

  ## Pros
  - 上一版 computed 的好處皆有
  - 預設 column 的排列位置清楚
  - 程式碼行數減少

  ## Cons
  - 條件更複雜情況下，連續使用三元運算會造成閱讀困難

</div>

<style>
.col-header {
  height: fit-content;
}
.two-cols-header {
  grid-template-rows: auto;
}
</style>

---

# 這次的需求最後可以寫成...

```vue {all|3-7|8-16|15,20,26,32|34-38|all} {lines:true, startLine:1, maxHeight: '55vh'}
<script setup>
const headerList = computed(() => [
  ...showCardIndex.value ? [{
    text: t('BROADCAST.CARD_INDEX'),
    value: 'cardIndex',
    width: '14%'
  }] : [],
  ...showActionIndex.value ? [{
    text: t('RICHMENU.DEFINE_ACTION'),
    value: 'actionIndex',
    width: '14%'
  }] : [{
    text: t('CHANNEL_PRESET_MESSAGES.BUTTON_NAME'),
    value: 'buttonName',
    width: showCardIndex.value ? '24%' : '28%'
  }],
  {
    text: t('CHANNEL_PRESET_MESSAGES.BUTTON_TYPE'),
    value: 'buttonType',
    width: showCardIndex.value ? '16%' : showActionIndex.value ? '22%' : '18%'
  },
  {
    text: t('BROADCAST.CLICK_RATE'),
    value: 'clickRate',
    align: 'end',
    width: showCardIndex.value ? '20%' : '26%'
  },
  {
    text: t('RICHMENU.CLICKS'),
    value: 'clicks',
    align: 'end',
    width: showCardIndex.value ? '26%' : showActionIndex.value ? '34%' : '28%'
  }
].map((item) => ({
  ...item,
  sortable: false,
  class: 'text--body-2 neutral-042--text'
})));
</script>
```
---
layout: center
---

# The end
