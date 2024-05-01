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

# Custom component / tool in Asgard

- Select input
- File previewer
- useRouteHash
- i18n sync CLI

---

# Select input

<div  class="flex items-center h-3/4 justify-center">
  <div class="w-1/2">
    <img src="/20240502/截圖 2024-05-01 下午3.10.05.png" >
  </div>
</div>
---

# Use

```vue
<script setup>
import SelectInput from '@/components/form/SelectInput.vue';

const optionList = [
  {
    text: '姓名',
    value: 'name'
  }
]
const searchParams = ref({
  type: 'name'
  input: ''
});
</script>

<template>
  <SelectInput
    v-model="params"
    :option-list="optionList"
    @change="changeHandler"
  />
</template>
```

---

# Component structure

```markdown {all}
SelectInput
 ┣ Select
 ┗ Input
```

```vue {all}{maxHeight:'300px'}

<template>
  <div
    class="d-flex custom-select-input"
  >
    <Select
      class="custom-select-input--select"
      :value="props.value.type"
      :items="props.optionList"
      item-text="text"
      item-value="value"
      :hide-details="true"
      @change="(value) => handleSelectChange(value)"
    />
    <Input
      :value="props.value.input"
      type="text"
      class="custom-select-input--text"
      clear-icon="mdi-close-circle"
      clearable
      :hide-details="true"
      :placeholder="$t('TEAMMATE.PLEASE_SEARCH')"
      @change="(value) => handleChange({ key:'input', value })"
    />
  </div>
</template>

<style lang="scss" scoped>
.custom-select-input {
  height: 32px;
  border: 1px solid $neutral-cold-053;
  border-radius: 4px;
  box-sizing: border-box;

  &:has(:focus) {
    transition: 0.3s cubic-bezier(0.25, 0.8, 0.5, 1);
    border-color: $primary-001;
    box-shadow: 0px 0px 0px 2px rgba(64, 143, 255, 0.25);
  }
}
...
</style>

```

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

# Component structure

```markdown {2}
FilePreviewer
 ┗ PdfPreviewer.vue
   ┗ ViewerContainer
```

```vue {all}{maxHeight:'300px'}
<script setup>
import { pdfjs } from '@/utils/pdfjs.js';

const getDocument = async () => {
  try {
    if (!props.filePath) {
      return;
    }
    isLoading.value = true;
    const file = await pdfjs.getDocument(props.filePath).promise;
    pdfDocument.value = file;

    const page = await file.getPage(1);
    const viewport = page.getViewport({ scale: 1 });
    const context = pdfContainer.value.getContext('2d');

    pdfContainer.value.height = viewport.height;
    pdfContainer.value.width = viewport.width;

    page.render({
      canvasContext: context,
      viewport
    });
  } catch (error) {
    console.error('pdf read error:', error);
  } finally {
    isLoading.value = false;
  }
};
</script>

<template>
  <div>
    <ViewerContainer :action-list="actionList">
      <template #previewPanel>
        <canvas ref="pdfContainer"/>
      </template>
    </ViewerContainer>
  </div>
</template>

```

---
layout: iframe-right

# the web page source
url: https://mozilla.github.io/pdf.js/

---

# utils/pdfjs.js

```js
import * as pdfjs from 'pdfjs-dist';

pdfjs.GlobalWorkerOptions.workerSrc = new URL(
  'pdfjs-dist/build/pdf.worker.js',
  import.meta.url
);

export { pdfjs };

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
