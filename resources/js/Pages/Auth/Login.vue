<script setup>
import { Head, Link, useForm } from "@inertiajs/inertia-vue3";
import AuthenticationCard from "@/Components/AuthenticationCard.vue";
import AuthenticationCardLogo from "@/Components/AuthenticationCardLogo.vue";
import Checkbox from "@/Components/Checkbox.vue";
import InputError from "@/Components/InputError.vue";
import InputLabel from "@/Components/InputLabel.vue";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import TextInput from "@/Components/TextInput.vue";

defineProps({
  canResetPassword: Boolean,
  status: String,
});

const form = useForm({
  email: "",
  password: "",
  remember: false,
});

const submit = () => {
  form
    .transform((data) => ({
      ...data,
      remember: form.remember ? "on" : "",
    }))
    .post(route("login"), {
      onFinish: () => form.reset("password"),
    });
};
</script>

<template>
  <q-form @submit="submit()"
    class="min-h-screen flex flex-col sm:justify-center items-center pt-6 sm:pt-0 bg-gray-100"
  >
    <q-card class="mx-auto w-96">
      <q-card-section class="flex">
        <div class="my-4 mx-auto">
          <AuthenticationCardLogo />
        </div>
      </q-card-section>
      <q-card-section>
        <div class="q-gutter-md w-full">
          <q-input
            class="w-full"
            :error="Boolean(form.errors.email)"
            :error-message="form.errors.email"
            type="text"
            label="Email Address"
            v-model="form.email"
            outlined
            dense
          />
          <q-input
            class="w-full"
            :error="Boolean(form.errors.password)"
            :error-message="form.errors.password"
            type="password"
            label="Password"
            v-model="form.password"
            outlined
            dense
          />
          <q-checkbox label="Remember me" dense v-model="form.remember" />
        </div>
      </q-card-section>
      <q-separator />
      <q-card-actions>
        <a size="sm" v-if="canResetPassword" :href="route('password.request')">
          Forgot your password?
        </a>
        <q-space />
        <q-btn type="submit" unelevated color="primary" :loading="form.processing">Login</q-btn>
      </q-card-actions>
    </q-card>
  </q-form>
</template>
