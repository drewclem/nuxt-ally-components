<template>
  <li>
    <template v-if="menu.submenus">
      <BaseMenuItem :menu="menu" :depth="depth + 1" />
    </template>
    <nuxt-link
      :id="menu.title.toLowerCase().replace(' ', '-')"
      v-else
      :to="menu.link"
    >
      {{ menu.title }}
    </nuxt-link>
  </li>
</template>

<script>
export default {
  props: {
    menu: {
      type: Object,
      required: true,
    },
    depth: {
      type: Number,
      required: true,
    },
  },
};
</script>
