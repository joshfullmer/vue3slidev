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

- Composition API
- Other new features
    - Teleport
    - Fragments (multiple root elements)
- Performance
- First-class Typescript support

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
  <AppMain />
  <AppFooter />
</template>
```

</div>

</div>

---
layout: section
---

# Composition API

---

# Example Component

```html
<template>
    <button @click="increment">+</button>
    <span>Count^2 {\{ countSquared }}</span>
</template>
```

<div v-click class="pt-4">
Multiple root nodes! 🎉
</div>

---

# Options API vs. Composition API

<div class="grid grid-cols-2 gap-4">

<div>

### Options API

```ts {all|2-4|10-14|5-9|all}
export default {
    data() {
        return { count: 0 }
    },
    computed: {
        countSquared() {
            return this.count ** 2
        }
    },
    methods: {
        increment() {
            this.count += 1
        }
    }
}
```

</div>

<div>

### Composition API

```ts {all|4|5|6-8|all}
import { computed, ref } from 'vue'
export default {
    setup() {
        const count = ref(0)
        const increment = () => count.value += 1
        const countSquared = computed(
            () => count.value ** 2
        )

        return {
            count,
            countSquared,
            increment
        }
    }
}
```

</div>

</div>

---
layout: section
---

# Sure, that's great, it can do the same thing.
# What makes it better?

---

# Composables

Consider a pattern that is used frequently: managing tooltip behavior.

<div class="grid grid-cols-2 gap-4">

<div>

```ts
export default {
    data() {
        return { isTooltipOpen: false }
    },
    methods() {
        toggleTooltip() {
            this.isTooltipOpen = !this.isTooltipOpen
        }
    }
}
```

</div>

<div>

```ts
// tooltip.js
export const useTooltip = () => {
    const isOpen = ref(false)
    const toggle = () => isOpen.value = !isOpen.value

    return { isOpen, toggle }
}

// Component.vue
import useTooltip from './tooltip'
export default {
    setup() {
        const { isOpen, toggle } = useTooltip()

        // Other component logic

        return { isOpen, toggle }
    }
}
```

</div>

</div>

---

# Separation of concerns

Imagine a component that has a header with a tooltip and also needs to render a list of things based on an API call.

<div class="grid grid-cols-2 gap-4">

<div>

### Options API

```ts
data() {
    return {
        isTooltipOpen: false,
        loading: false,
        items: []
    }
}
```

```ts
computed: {
    sortedItems() { ... }
    tooltipTitle() { ... }
}
```

```ts
methods: {
    async loadItems() { ... }
    toggleTooltip() { ... }
}
```

</div>

<div>

### Composition API

```ts
setup() {
    // ITEMS
    const loading = ref(false)
    const items = ref([])
    const sortedItems = computed(() => { ... })
    const loadItems = () => { ... }
    mounted(() => loadItems())

    // TOOLTIP
    const isTooltipOpen = ref(false)
    const tooltipTitle = computed(() => { ... })
    const toggleTooltip = () => { ... }

    // Only return the items used in the template
    // e.g. Don't return loadItems or items
    return { ... }
}
```

</div>

</div>

---

# `<script setup>`

[more info](https://github.com/vuejs/rfcs/blob/script-setup-2/active-rfcs/0000-script-setup.md)

```ts
<script setup>
import { ref, computed } from 'vue'

const count = ref(0)
const countDoubled = computed(() => count.value * 2)
const increment = () => count.value += 1
</script>
```

All of these constants become available in the template.

---
layout: center
---

# Better Vue Tooling

- Enhanced Vue DevTools extension
- Better intelliense
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
layout: section
---

# First-class Typescript support

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

# Type safety for event emissions

Consider a component that emits a value that is selected by the user

```ts
export default defineComponent({
    emits: {
        select(payload: { value: string; label: string; }) { ... }
    },
    setup(props, { emit }) {
        const onClick = (() => {
            emit('select', { somethingElse: 0 }) // throws compilation error
        })

        return { onClick }
    }
})
```

---
layout: section
---

# Great, sign me up!

---

# Migration

1. Remove [deprecated](https://v3.vuejs.org/guide/migration/migration-build.html#preparations) [syntax](https://vuejs.org/v2/guide/components-slots.html#Deprecated-Syntax) for slots
1. Upgrade to compat version (Vue 3.1)
1. Resolve [incompatible](https://v3.vuejs.org/guide/migration/migration-build.html#incompatible) and [partially compatible](https://v3.vuejs.org/guide/migration/migration-build.html#partially-compatible-with-caveats) issues
1. Upgrade related dependencies (vuex, vue-router, vue-i18n)
1. Refactor app and components over time to remove features removed from Vue 3
1. Update eslint plugins to vue3 plugins

[Full migration guide](https://v3.vuejs.org/guide/migration/migration-build.html#overview)

[Additional information for Keap's migration](https://docs.google.com/document/d/1fqBDxTSLfthQ-Ts9g65S1bsxuREjo49snlbFK4iEAj4/edit)
