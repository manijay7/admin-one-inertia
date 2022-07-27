<script setup>
import { ref } from "vue";
import {
  mdiAccount,
  mdiAccountCircle,
  mdiLock,
  mdiMail,
  mdiAsterisk,
  mdiFormTextboxPassword,
  mdiPhoneOutline,
} from "@mdi/js";
import SectionMain from "@/components/SectionMain.vue";
import CardBox from "@/components/CardBox.vue";
import SectionTitleBar from "@/components/SectionTitleBar.vue";
import BaseDivider from "@/components/BaseDivider.vue";
import FormField from "@/components/FormField.vue";
import FormControl from "@/components/FormControl.vue";
import BaseButton from "@/components/BaseButton.vue";
import BaseButtons from "@/components/BaseButtons.vue";
import { useForm, usePage } from "@inertiajs/inertia-vue3";
import { computed } from "@vue/reactivity";

const titleStack = ref(["Users", "Profile"]);

const passwordForm = useForm({
  password: "",
  password_confirmation: "",
});

const submitPass = () => {
  passwordForm.post(route("users.change-password"));
};
</script>

<template>
  <SectionTitleBar :title-stack="titleStack" />
  <SectionMain>
    <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
      <CardBox
        title="Change Password"
        :icon="mdiLock"
        form
        @submit.prevent="submitPass"
      >
        <FormField label="New password">
          <FormControl
            v-model="passwordForm.password"
            :icon="mdiFormTextboxPassword"
            name="password"
            type="password"
            required
            autocomplete="new-password"
            :error="passwordForm.errors.password"
          />
        </FormField>

        <FormField label="Confirm password">
          <FormControl
            v-model="passwordForm.password_confirmation"
            :icon="mdiFormTextboxPassword"
            name="password_confirmation"
            type="password"
            required
            autocomplete="new-password"
            :error="passwordForm.errors.password_confirmation"
          />
        </FormField>

        <BaseDivider />

        <BaseButtons>
          <BaseButton type="submit" color="info" label="Submit" />
          <BaseButton color="danger" label="Clear" outline />
        </BaseButtons>
      </CardBox>
    </div>
  </SectionMain>
</template>
