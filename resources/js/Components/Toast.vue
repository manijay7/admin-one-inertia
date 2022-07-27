<script setup>
import { ref, computed, useSlots, watch, onMounted } from "vue";
import { colorsBg, colorsBorders, colorsOutline } from "@/colors.js";
import BaseLevel from "@/Components/BaseLevel.vue";
import BaseIcon from "@/Components/BaseIcon.vue";
import BaseButton from "@/Components/BaseButton.vue";
import { mdiClose } from "@mdi/js";
import { usePage } from "@inertiajs/inertia-vue3";

const props = defineProps({
  icon: {
    type: String,
    default: null,
  },
  outline: Boolean,
  notification: Object,
});

const componentClass = computed(() =>
  props.outline
    ? colorsOutline[props.notification.type]
    : [
        colorsBg[props.notification.type],
        colorsBorders[props.notification.type],
      ]
);

const show = ref(true);

const dismiss = () => {
  show.value = false;
};

const slots = useSlots();

onMounted(() => console.log(props.notification));

const hasRightSlot = computed(() => slots.right);

watch(
  () => props.notification,
  () => {
    // console.log(props.notification);
    show.value = true;
    setTimeout(() => (show.value = false), 3000);
  },
  { deep: true }
);
</script>

<template>
  <div
    v-if="$page.props.flash.notification && show"
    :class="componentClass"
    class="flex flex-col w-full px-2 py-3 md:py-3 mx-6 md:mx-0 mb-6 last:mb-0 border rounded transition-colors duration-150 shadow dark:text-gray-400 dark:bg-gray-800"
  >
    <div class="flex justify-between">
      <BaseLevel>
        <div class="flex md:flex-row items-center">
          <BaseIcon
            v-if="icon"
            :path="icon"
            w="w-10 md:w-5"
            h="h-10 md:h-5"
            size="24"
            class="md:mr-2"
          />
          <span class="text-center md:text-left">{{
            $page.props.flash.notification.title
          }}</span>
        </div>
        <slot v-if="hasRightSlot" name="right" />
        <BaseButton
          v-else
          :icon="mdiClose"
          :outline="outline"
          small
          @click="dismiss"
        />
      </BaseLevel>
    </div>
    <slot />
  </div>
</template>
