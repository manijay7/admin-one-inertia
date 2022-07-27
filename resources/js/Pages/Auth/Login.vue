<script>
import Guest from "@/Layouts/Guest.vue";
export default {
  layout: Guest,
};
</script>

<script setup>
import { useLayoutStore } from "@/stores/layout.js";
import { useForm, Head, Link } from "@inertiajs/inertia-vue3";
import { mdiAccount, mdiAsterisk } from "@mdi/js";
import SectionFullScreen from "@/Components/SectionFullScreen.vue";
import CardBox from "@/Components/CardBox.vue";
import FormField from "@/Components/FormField.vue";
import FormControl from "@/Components/FormControl.vue";
import BaseDivider from "@/Components/BaseDivider.vue";
import BaseButton from "@/Components/BaseButton.vue";
import BaseButtons from "@/Components/BaseButtons.vue";
import FormValidationErrors from "@/Components/FormValidationErrors.vue";
import NotificationBarInCard from "@/Components/NotificationBarInCard.vue";
import BaseLevel from "@/Components/BaseLevel.vue";
import BreezeValidationErrors from "@/Components/ValidationErrors.vue";
import { mdiLoading } from "@mdi/js";
const props = defineProps({
  canResetPassword: Boolean,
  status: {
    type: String,
    default: null,
  },
});

const form = useForm({
  email: "",
  password: "",
  remember: [],
});

useLayoutStore().fullScreenToggle(true);

const submit = () => {
  form
    .transform((data) => ({
      ...data,
      remember: form.remember && form.remember.length ? "on" : "",
    }))
    .post(route("login"), {
      onFinish: () => form.reset("password"),
    });
};
</script>

<template>
  <Head title="Login" />

  <SectionFullScreen v-slot="{ cardClass, cardRounded }" bg="login">
    <CardBox
      :class="cardClass"
      :rounded="cardRounded"
      form
      @submit.prevent="submit"
    >
      <div class="flex justify-center items-center">
        <h3 class="mb-3 text-2xl font-bold">Login</h3>
      </div>
      <BreezeValidationErrors class="mb-4" />

      <FormField label="Email" label-for="email" help="Please enter your email">
        <FormControl
          v-model="form.email"
          :icon="mdiAccount"
          id="email"
          autocomplete="email"
          type="email"
          required
        />
      </FormField>

      <FormField
        label="Password"
        label-for="password"
        help="Please enter your password"
      >
        <FormControl
          v-model="form.password"
          :icon="mdiAsterisk"
          type="password"
          id="password"
          autocomplete="current-password"
          required
        />
      </FormField>

      <!-- <FormCheckRadioPicker
        v-model="form.remember"
        name="remember"
        :options="{ remember: 'Remember' }"
      /> -->

      <BaseDivider />

      <BaseLevel>
        <BaseButtons>
          <BaseButton
            type="submit"
            color="info"
            :label="form.processing ? '...' : 'Login'"
            :class="{ 'opacity-25': form.processing }"
            :disabled="form.processing"
          />
          <BaseButton
            v-if="canResetPassword"
            route-name="password.request"
            color="info"
            outline
            label="Remind"
          />
        </BaseButtons>
      </BaseLevel>
    </CardBox>
  </SectionFullScreen>
</template>
