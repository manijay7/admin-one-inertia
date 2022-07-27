<script setup>
import { computed } from "vue";
import { useMainStore } from "@/stores/main.js";
import { useLayoutStore } from "@/stores/layout.js";
import menu from "@/menu.js";
import FooterBar from "@/Components/FooterBar.vue";
import OverlayLayer from "@/Components/OverlayLayer.vue";
import NavBar from "@/Components/NavBar.vue";
import AsideMenu from "@/Components/AsideMenu.vue";
import { mdiSelectColor, mdiFeather } from "@mdi/js";
import Toast from "@/Components/Toast.vue";

const mainStore = useMainStore();

const layoutStore = useLayoutStore();
layoutStore.fullScreenToggle(false);
const isAsideLgActive = computed(() => layoutStore.isAsideLgActive);

const overlayClick = () => {
    layoutStore.asideLgToggle(false);
};
</script>

<template>
    <NavBar />
    <AsideMenu :menu="menu" />

    <div class="absolute right-0 top-16 mr-8 max-w-sm z-50">
        <!-- <Toast
      :icon="mdiFeather"
      :notification="$page.props.flash.notification"
      scroll-region
    >
      {{ $page.props.flash.message }}
    </Toast> -->
    </div>

    <slot />
    <FooterBar />
    <OverlayLayer
        v-show="isAsideLgActive"
        z-index="z-30"
        @overlay-click="overlayClick"
    />
</template>
