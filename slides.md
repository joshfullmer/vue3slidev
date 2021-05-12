---
download: true
title: Vue 3 at Keap
highlighter: shiki
---

<div class="flex items-center">
  <div class="w-[60%]">
    <h1>Vue 3 at Keap</h1>
    <p>Making the world a better place by keeping up with modern frontend frameworks /s</p>
  </div>
  <div class="flex items-center gap-8 ml-auto">
    <img src="/keap.svg" class="h-[100px]" />
    <mdi-plus class="text-2xl" />
    <img src="/vue.svg" class="h-[100px]" />
  </div>
</div>

---
layout: center
---

# New to Vue 3

- First-class Typescript support
- Performance
- Composition API
- Other new features
    - Teleport
    - Fragments (multiple root elements)

---
layout: center
---

# Why Typescript?

- More reliable/higher confidence in code
- Typing acts as documentation
- Better tooling/intellisense
- Easier to maintain with high amount of developers

---
layout: section
---

# Sure, but why Typescript in Vue?

---

# Better type safety for props

<div class="grid grid-cols-2">

<div>

### Vue 2

```ts {all|3-4|all}
props: {
  broadcast: {
    type: Object,
    required: true,
    // default: () => ({}),
  },
}
```

<v-click>

```ts {5-9|all}
props: {
  broadcast: {
    type: Object,
    required: true,
    validator: (value) => {
      ['id', 'scheduleDate'].forEach((key) => {
        broadcast.hasOwnProperty(key)
      })
    },
    // But what about the type of the values in broadcast?
  },
}
```

</v-click>

</div>

<div>

### Vue 3 + Typescript

<v-click>

```ts {all|3-6|10|all}
import { PropType } from 'vue'

type Broadcast = {
  id: string | number;
  scheduleDate: string;
}

props: {
  broadcast: {
    type: Object as PropType<Broadcast>,
    required: true,
  }
}
```

</v-click>

<v-click>

```ts
// Using <script setup> syntactic sugar
type Broadcast = {
  id: string | number;
  scheduleDate: string;
}

const { broadcast } = defineProps<{
  broadcast: Broadcast
}>()
```

</v-click>

</div>

</div>

---
layout: center
---

# Better Vue Tooling

- [Vetur](https://github.com/vuejs/vetur)
- [Volar](https://marketplace.visualstudio.com/items?itemName=johnsoncodehk.volar)

---
layout: center
---

# Performance

- Smaller bundle size due to treeshakeability
- Mounts quicker
- Updates quicker
- Uses less memory
- [Spreadsheet with details](https://docs.google.com/spreadsheets/d/1VJFx-kQ4KjJmnpDXIEaig-cVAAJtpIGLZNbv3Lr4CR0/edit#gid=0)

---

# New features

- Teleport
    - Similar to `portal-vue`, can move elements/components through the DOM
- Fragments
    - Multiple root elements in component
- Composition API
- [Many others](https://v3.vuejs.org/guide/migration/introduction.html#notable-new-features)

---

# Fragments example

<div class="grid grid-cols-2 gap-4">

<div>

### Before

```html {all|2,6|all}
<template>
  <div>
    <AppHeader />
    <AppMain />
    <AppFooter />
  </div>
</template>
```

</div>

<div v-click>

### After

```html
<template>
  <AppHeader />
  <AppMain v-bind="$attrs" />
  <AppFooter />
</template>
```

NOTE: You need to define which top level element inherits attributes

</div>

</div>

---
layout: statement
---

# Composition API

---

# Migration

[Link](https://github.com/vuejs/vue-next/tree/master/packages/vue-compat)
