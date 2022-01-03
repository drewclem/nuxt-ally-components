<template>
  <div class="site-wrapper">
    <SiteHeader :inert="pageHasModalOpen" />
    <main :inert="pageHasModalOpen">
      <SidebarNav />
      <nuxt />
    </main>
    <SiteFooter :inert="pageHasModalOpen" />

    <PortalTarget name="modal"></PortalTarget>
  </div>
</template>

<script>
import { mapState } from "vuex";

export default {
  computed: {
    ...mapState("modal", ["pageHasModalOpen"]),
  },
};
</script>

<style>
body {
  overflow-x: hidden;
  margin: 0 !important;
}

.site-wrapper {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

header,
footer {
  flex-grow: 0;
}

main {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-grow: 1;
}
</style>
