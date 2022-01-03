<template>
  <nav id="Sidebar Navigation" @keydown="handleKeyPress">
    <ul>
      <BaseMenu
        v-for="(menu, index) in navigation"
        :menu="menu"
        :key="index"
        :depth="0"
      />
    </ul>
  </nav>
</template>

<script>
export default {
  data() {
    return {
      navigation: [
        {
          title: "Menu 1",
          link: "/",
        },
        {
          title: "Menu 2",
          submenus: [
            {
              title: "Submenu 1",
              link: "/",
            },
            {
              title: "Submenu 2",
              link: "/",
            },
            {
              title: "Submenu 3",
              submenus: [
                {
                  title: "Subsubmenu 1",
                  link: "/",
                },
                {
                  title: "Subsubmenu 2",
                  link: "/",
                },
              ],
            },
          ],
        },
      ],
    };
  },
  methods: {
    handleKeyPress(e) {
      // Query to get focusable elements within the sidebar nav
      const navEl = document.getElementById("Sidebar Navigation");
      const focusableElements = navEl.querySelectorAll(["a", "button"]);
      // Convert nodelist of elements into an array
      const focusableElementsArr = Array.from(focusableElements);
      // Grab current active element
      const activeEl = document.activeElement;
      // Find current active element within the array of focusable elements
      const activeElIndex = focusableElementsArr.findIndex(
        (f) => f.id === activeEl.id
      );
      const lastIdx = focusableElementsArr.length - 1;
      if (e.key === "ArrowUp") {
        activeElIndex <= 0
          ? focusableElementsArr[0].focus()
          : focusableElementsArr[activeElIndex - 1].focus();
      }
      if (e.key === "ArrowDown") {
        activeElIndex >= lastIdx
          ? focusableElementsArr[lastIdx].focus()
          : focusableElementsArr[activeElIndex + 1].focus();
      }
    },
  },
};
</script>
