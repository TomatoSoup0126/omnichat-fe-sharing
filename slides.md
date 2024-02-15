---
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
title: FE Sharing 2024.02.15
mdc: true
---

# Sharing 2024.02.15

Jonathan

---
layout: center
---

# 購物車再行銷的客製化 Notification

---

# useNotification.js

```js {all|2|3|4-10|12-15|18-21} {lines:true, startLine:1}
// ...
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
export {
  defineNotification,
  useNotification
};
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
