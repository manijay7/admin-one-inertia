<script setup>
import { computed } from "vue";
import { useMainStore } from "@/stores/main.js";
import { useLayoutStore } from "@/stores/layout.js";
import menu from "@/menu.js";

const mainStore = useMainStore();

const layoutStore = useLayoutStore();
layoutStore.fullScreenToggle(true);
</script>

<template>
  <slot />
</template>
