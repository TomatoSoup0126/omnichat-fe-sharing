---
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
title: FE Sharing 2024.05.02
mdc: true
monaco: true
---

# Sharing 2024.05.12

Jonathan

---
layout: center
---

# Custom component in Asgard

- File previewer
- Select input

---

# File Previewer

<div  class="flex items-center h-3/4">
  <div class="w-1/2">
    <img src="/20240502/截圖 2024-04-28 上午10.46.13.png" >
  </div>
  <div class="w-1/2">
    <img src="/20240502/截圖 2024-04-28 上午10.47.18.png" >
  </div>
</div>

---

# Use

```vue
<script setup>
import FilePreviewer from '@/components/filePreviewer/FilePreviewer.vue';

const filePath = ref('')
const height = ref(200)
const showDownloadButton = ref(false)
</script>

<template>
  <FilePreviewer
    :file-path="filePath"
    :height="height"
    :show-download-button="showDownloadButton"
  />
</template>
```
---
layout: iframe

# the web page source
url: http://localhost:8080/design-guideline-sample
---

---

# Component structure

```markdown {all|1|2,5}
FilePreviewer
 ┣ ImagePreviewer.vue
 ┃ ┣ ViewerContainer
 ┃ ┗ LightboxDialog
 ┗ PdfPreviewer.vue
   ┗ ViewerContainer
```
<v-clicks>
```vue
<script setup>
// ...
const isPdfFile = computed(() => props.filePath.endsWith('.pdf'));
</script>
<template>
  <div>
    <PdfPreviewer
      v-if="isPdfFile"
      v-bind="$props"
    />
    <ImagePreviewer
      v-else
      v-on="$listeners"
      v-bind="$props"
    />
  </div>
</template>

```
</v-clicks>

---

# Component structure

```markdown {2}
FilePreviewer
 ┗ ImagePreviewer.vue
   ┣ ViewerContainer
   ┗ LightboxDialog
```

```vue {all}{maxHeight:'300px'}
<script setup>
const showLightbox = ref(false);

const actionList = computed(() => ([
  {
    type: 'expand',
    icon: 'expand',
    action: () => {
      showLightbox.value = true;
    }
  },
  {
    type: 'download',
    icon: 'export_download',
    action: () => {
      downloadImageFromUrl(props.filePath);
    }
  }
]));
</script>
<template>
  <ViewerContainer :action-list="actionList">
    <template #previewPanel>
      <img :src="props.filePath">
    </template>
  </ViewerContainer>
  <LightboxDialog
    :file-path="props.filePath"
    :display="showLightbox"
    @close="showLightbox = false"
  />
</template>

```

---

# Component structure

```markdown {3}
FilePreviewer
 ┗ ImagePreviewer.vue
   ┣ ViewerContainer
   ┗ LightboxDialog
```

```vue {all}{maxHeight:'300px'}
<template>
  <v-sheet
    class="viewer-container"
    :style="{'--height': `${props.height}px`}"
    width="100%"
    height="100%"
  >
    <div
      class="action-panel"
      v-if="hasActionList"
    >
      <SvgIcon
        v-for="action in props.actionList"
        class="pointer-cursor"
        :key="action.type"
        :icon-class="action.icon"
        size="32"
        color="#FFFFFF"
        @click="action.action"
      />
    </div>
    <div class="preview-panel">
      <slot name="previewPanel" />
    </div>
  </v-sheet>
</template>

```

---

# Component structure

```markdown {4}
FilePreviewer
 ┗ ImagePreviewer.vue
   ┣ ViewerContainer
   ┗ LightboxDialog
```

```vue {all}{maxHeight:'300px'}
<template>
  <v-dialog
    class="lightbox-dialog"
    :value="props.display"
    fullscreen
    hide-overlay
    @keydown="close"
  >
    <div class="mask" />
    <div class="action-container">
      <div
        class="action-btn"
        @click="downloadImageFromUrl(props.filePath)"
      >
        <SvgIcon
          v-if="props.showDownloadButton"
          icon-class="export_download"
          size="40"
          color="#FFFFFF"
        />
      </div>
      <div
        class="action-btn"
        @click="close"
      >
        <SvgIcon
          class="pointer-cursor"
          icon-class="cancel"
          size="40"
          color="#FFFFFF"
        />
      </div>
    </div>
    <div class="preview-img">
      <img
        :src="props.filePath"
        alt="preview"
        width="100%"
        height="100%"
      >
    </div>
  </v-dialog>
</template>

```

---

# AppLayout.vue

```vue {all|3,7,10|8|12-14} {lines:true, startLine:1}
<template>
  <!-- ... -->
  <Notification ref="notificationRef" />
</template>

<script setup>
import Notification from '@/components/Notification.vue';
import { defineNotification } from '@/components/composables/useNotification';

const notificationRef = ref(null);

onMounted(() => {
  defineNotification(notificationRef);
});
</script>
```

---

# Vue SFC

```vue {all|2|4|6-11,15-18} {lines:true, startLine:1}
<script setup>
import { useNotification } from '@/components/composables/useNotification';

const { openNotification } = useNotification();

const showNotification = () => {
  openNotification({,
    status: 'success'
    title: '我是標題'
    content: '我是內容'
  });
};
</script>

<template>
  <Button @click="showNotification">
    跳個通知
  </Button>
</template>
```
---

# 購物車再行銷的客製化 Notification

<div  class="flex justify-center">
  <img src="/20240215/notification_with_footer.png" class="w-auto">
</div>

---
layout: two-cols-header
---

# Notification.vue

::left::

```vue {all|5,10,14|20} {lines:true, startLine:1}
<template>
  <v-snackbar>
    <div class="d-flex align-start text--subtitle-2">
      <v-icon>
        {{ notificationTitleIcon }}
      </v-icon>
      <slot name="custom">
        <div class="ml-4">
          <p class="mb-0 text--subtitle-3 neutral-040--text">
            {{ notificationTitle }}
          </p>
          <p
            class="notification-content"
            v-html="notificationContent"
          />
        </div>
      </slot>
      <!-- ... -->
    </div>
    <slot name="footer" />
  </v-snackbar>
</template>
```

::right::

```vue {0|all} {lines:true, startLine:1, at:0}
<script>
export default {
  methods: {
    openNotification({
      title = '', content = '', status = 'error'
    } = {}) {
      this.resetNotification();

      this.notificationStatus = status;
      this.notificationTitle = title;
      this.notificationContent = content;

      this.isShowNotification = true;
    },
  }
}
</script>

```

---

# useNotification.js

```js {monaco-diff}
const defineNotification = (notificationRef) => {
  const notification = ref(notificationRef);
  useNotification = function () {
    function openNotification(obj) {
      notification.value.openNotification(obj);
    }
    function closeNotification() {
      notification.value.closeNotification();
    }
    onScopeDispose(closeNotification);
    return {
      openNotification,
      closeNotification
    };
  };
};
~~~
const defineNotification = (notification) => {
  const notificationRef = ref(notification);
  useNotification = function () {
    function openNotification(obj) {
      notification.value.openNotification(obj);
    }
    function closeNotification() {
      notification.value.closeNotification();
    }
    onScopeDispose(closeNotification);
    return {
      openNotification,
      closeNotification,
      notificationRef
    };
  };
};
```

---

# Render function to footer slot

```vue {all|2,7-17|3,5|19|7-17,20|21-24} {lines:true, startLine:1, maxHeight:'400px'}
<script setup>
import { h } from 'vue';
import { useNotification } from '@/components/composables/useNotification';

const { openNotification, notificationRef } = useNotification();

const linkToSetting = h(
  'a',
  {
    attrs: {
      href: `${import.meta.env.VITE_APP_CHAT_ADMIN_URL}/product-feed.html`,
      class: 'primary-001--text text--body-2 w-full text-right mt-4',
      style: 'display: block;'
    }
  },
  '我是被塞進來的 footer link'
);

const showNotification = () => {
  notificationRef.value.$slots.footer = linkToSetting;
  openNotification({
    title: '我是 title',
    content: '我是 content'
  });
};
</script>
```

---

# Reset footer slot

```vue {monaco-diff}
<script>
const showNotification = () => {
  notificationRef.value.$slots.footer = linkToSetting;
  openNotification({
    title: '我是 title',
    content: '我是 content'
  });
};
</script>
~~~
<script>
const showNotification = () => {
  notificationRef.value.$slots.footer = linkToSetting;
  openNotification({
    title: '我是 title',
    content: '我是 content'
  });
  nextTick(() => {
    notificationRef.value.$slots.footer = null;
  });
};
</script>
```

---
layout: center
---

# The end
