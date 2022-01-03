<template>
  <div>
    <button
      @click.prevent="toggleMenu(menu)"
      :id="menuId"
      aria-haspopup="true"
      ref="menuButtonRef"
      :aria-expanded="isOpen.toString()"
      :aria-controls="submenuId"
    >
      {{ menu.title }}
    </button>

    <ul v-show="isOpen" @keydown.esc.stop="closeMenu" :id="submenuId">
      <BaseMenu
        v-for="(menu, index) in menu.submenus"
        :key="index"
        :menu="menu"
        :depth="depth + 1"
      />
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isOpen: false,
    };
  },
  props: {
    depth: {
      type: Number,
      required: true,
    },
    menu: {
      type: Object,
      required: true,
    },
  },
  computed: {
    menuId() {
      return this.menu.title.toLowerCase().replace(" ", "-");
    },
    submenuId() {
      return `${this.menu.title.toLowerCase().replace(" ", "-")}-submenu`;
    },
  },
  methods: {
    toggleMenu() {
      this.isOpen = !this.isOpen;

      if (this.isOpen) {
        this.$nextTick(() => {
          const submenu = document.getElementById(this.submenuId);
          const firstItem = submenu.querySelector("a, button");
          firstItem?.focus();
        });
      }

      if (!this.isOpen) {
        this.$nextTick(() => {
          this.$refs.menuButtonRef?.focus();
        });
      }
    },
    closeMenu() {
      this.isOpen = false;

      this.$refs.menuButtonRef?.focus();
    },
  },
};
</script>
