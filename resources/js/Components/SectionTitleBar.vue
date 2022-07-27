<script setup>
import { mdiGithub } from "@mdi/js";
import BaseLevel from "@/components/BaseLevel.vue";
import BaseButton from "@/components/BaseButton.vue";

defineProps({
  titleStack: {
    type: Array,
    default: () => [],
  },
});
</script>

<template>
  <section class="px-6 py-2">
    <BaseLevel mobile>
      <ul>
        <li
          v-for="(title, index) in titleStack"
          :key="index"
          class="stack-item inline-block pr-3 text-lg text-gray-500 dark:text-gray-400 last:pr-0 last:font-black last:text-black"
        >
          {{ title }}
        </li>
      </ul>
      <slot />
    </BaseLevel>
  </section>
</template>
