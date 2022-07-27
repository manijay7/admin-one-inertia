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
import SectionFullScreen from "@/components/SectionFullScreen.vue";
import CardBox from "@/components/CardBox.vue";
import FormCheckRadioPicker from "@/components/FormCheckRadioPicker.vue";
import FormField from "@/components/FormField.vue";
import FormControl from "@/components/FormControl.vue";
import BaseDivider from "@/components/BaseDivider.vue";
import BaseButtons from "@/components/BaseButtons.vue";
import FormValidationErrors from "@/components/FormValidationErrors.vue";
import NotificationBarInCard from "@/components/NotificationBarInCard.vue";
import BaseLevel from "@/components/BaseLevel.vue";

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
    form.transform((data) => ({
        ...data,
        remember: form.remember && form.remember.length ? "on" : "",
    })).post(route("login"), {
        onFinish: () => form.reset("password"),
    });
};
</script>

<template>
    <!-- component -->
    <div class="bg-white dark:bg-gray-900">
        <div class="flex justify-center h-screen">
            <div
                class="hidden bg-cover lg:block lg:w-2/3"
                style="
                    background-image: url(https://source.unsplash.com/random/?software);
                "
            >
                <div
                    class="flex items-center h-full px-20 bg-gray-900 bg-opacity-40"
                >
                    <div>
                        <h2 class="text-4xl font-bold text-white">
                            IS EVOLUTIONS CRM
                        </h2>

                        <p class="max-w-xl mt-3 text-gray-300">
                            IS Evolutions Customer Relationship Management
                            System
                        </p>
                    </div>
                </div>
            </div>

            <div
                class="flex items-center w-full max-w-md px-6 mx-auto lg:w-2/6"
            >
                <div class="flex-1">
                    <div class="text-center">
                        <h2
                            class="text-4xl font-bold text-center text-gray-700 dark:text-white"
                        >
                            Welcome Back,
                        </h2>

                        <p class="mt-3 text-gray-500 dark:text-gray-300">
                            Sign in to access your account
                        </p>
                    </div>

                    <div class="mt-8">
                        <FormValidationErrors />
                        <form @submit.prevent="submit">
                            <div>
                                <label
                                    for="email"
                                    class="block mb-2 text-sm text-gray-600 dark:text-gray-200"
                                    >Email Address</label
                                >
                                <input
                                    type="email"
                                    v-model="form.email"
                                    id="email"
                                    required
                                    placeholder="example@example.com"
                                    class="block w-full px-4 py-2 mt-2 text-gray-700 placeholder-gray-400 bg-white border border-gray-200 rounded-md dark:placeholder-gray-600 dark:bg-gray-900 dark:text-gray-300 dark:border-gray-700 focus:border-blue-400 dark:focus:border-blue-400 focus:ring-blue-400 focus:outline-none focus:ring focus:ring-opacity-40"
                                />
                            </div>

                            <div class="mt-6">
                                <div class="flex justify-between mb-2">
                                    <label
                                        for="password"
                                        class="text-sm text-gray-600 dark:text-gray-200"
                                        >Password</label
                                    >
                                    <Link
                                        v-if="canResetPassword"
                                        :href="route('password.request')"
                                        class="text-sm text-gray-700 focus:text-blue-500 hover:text-blue-500 hover:underline"
                                        >Forgot password?</Link
                                    >
                                </div>

                                <input
                                    type="password"
                                    v-model="form.password"
                                    id="password"
                                    required
                                    placeholder="Your Password"
                                    class="block w-full px-4 py-2 mt-2 text-gray-700 placeholder-gray-400 bg-white border border-gray-200 rounded-md dark:placeholder-gray-600 dark:bg-gray-900 dark:text-gray-300 dark:border-gray-700 focus:border-blue-400 dark:focus:border-blue-400 focus:ring-blue-400 focus:outline-none focus:ring focus:ring-opacity-40"
                                />
                            </div>

                            <div class="mt-6">
                                <button
                                    :class="{ 'opacity-25': form.processing }"
                                    :disabled="form.processing"
                                    class="w-full px-4 py-2 tracking-wide text-white transition-colors duration-200 transform bg-blue-500 rounded-md hover:bg-blue-400 focus:outline-none focus:bg-blue-400 focus:ring focus:ring-blue-300 focus:ring-opacity-50"
                                >
                                    Sign in
                                </button>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<!-- <template>
  <Head title="Login" />

  <SectionFullScreen v-slot="{ cardClass, cardRounded }" bg="login">
    <CardBox
      :class="cardClass"
      :rounded="cardRounded"
      form
      @submit.prevent="submit"
    >
      <FormValidationErrors />

      <NotificationBarInCard v-if="status" color="info">
        {{ status }}
      </NotificationBarInCard>

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

      <FormCheckRadioPicker
        v-model="form.remember"
        name="remember"
        :options="{ remember: 'Remember' }"
      />

      <BaseDivider />

      <BaseLevel>
        <BaseButtons>
          <BaseButton
            type="submit"
            color="info"
            label="Login"
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
</template> -->
