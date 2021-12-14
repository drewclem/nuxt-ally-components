<template>
  <div>
    <button
      @click.prevent="toggleMenu(menu)"
      :id="menu.title.toLowerCase().replace(' ', '-')"
      aria-haspopup="true"
      ref="menuButtonRef"
      :aria-expanded="isOpen.toString()"
      :aria-controls="`${menu.title.toLowerCase().replace(' ', '-')}-submenu`"
    >
      {{ menu.title }}
    </button>

    <ul
      v-show="isOpen"
      @keydown.esc.stop="closeMenu"
      :id="`${menu.title.toLowerCase().replace(' ', '-')}-submenu`"
    >
      <BaseMenu
        v-for="(menu, index) in menu.submenus"
        :key="index"
        :menu="menu"
        :depth="depth + 1"
        :is-parent-open="isOpen"
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
    isParentOpen: {
      type: Boolean,
      default: false,
    },
  },
  watch: {
    isParentOpen: function () {
      if (!this.isParentOpen) {
        this.isOpen = false;
      }
    },
  },
  methods: {
    toggleMenu(menu) {
      // format menu name to match ID
      const activeMenu = `${menu.title}-submenu`
        .toLowerCase()
        .replace(" ", "-");

      this.isOpen = !this.isOpen;

      if (this.isOpen) {
        // $nextTick here to wait for the DOM to render the previously hidden UL before we can grab it and focus the first element
        this.$nextTick(() => {
          this.$nextTick(() => {
            const subMenu = document.getElementById(activeMenu);
            const firstItem = subMenu.querySelector("a, button");
            firstItem?.focus();
          });
        });
      }

      // Focus the initial menu trigger if closing a menu
      if (!this.isOpen) {
        this.$nextTick(() => {
          this.$nextTick(() => {
            this.$refs.menuButtonRef?.focus();
          });
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
