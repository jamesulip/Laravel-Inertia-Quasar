<script setup>
import { ref } from "vue";
import { useForm } from "@inertiajs/inertia-vue3";
import ActionMessage from "@/Components/ActionMessage.vue";
import FormSection from "@/Components/FormSection.vue";
import InputError from "@/Components/InputError.vue";
import InputLabel from "@/Components/InputLabel.vue";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import TextInput from "@/Components/TextInput.vue";

const passwordInput = ref(null);
const currentPasswordInput = ref(null);

const form = useForm({
  current_password: "",
  password: "",
  password_confirmation: "",
});

const updatePassword = () => {
  form.put(route("user-password.update"), {
    errorBag: "updatePassword",
    preserveScroll: true,
    onSuccess: () => form.reset(),
    onError: () => {
      if (form.errors.password) {
        form.reset("password", "password_confirmation");
        passwordInput.value.focus();
      }

      if (form.errors.current_password) {
        form.reset("current_password");
        currentPasswordInput.value.focus();
      }
    },
  });
};
const passwordRules = true
</script>

<template>
    <q-form class="mx-auto ">
      <q-card flat bordered>
        <q-card-section>
          <div class="text-h6"> Update Password</div>
          <div class="text-subtitle2">
            Ensure your account is using a long, random password to stay secure.
          </div>
        </q-card-section>
        <q-separator />
        <q-card-section>
            <div class="q-gutter-md w-full">
            <q-input type="password" :error="Boolean(form.errors.current_password)" :error-message="form.errors.current_password"  v-model="form.current_password" outlined label="Current Password" />
            <q-input type="password" :error="Boolean(form.errors.password)" :error-message="form.errors.password" v-model="form.password" outlined label="New Password" />
            <q-input type="password" :error="Boolean(form.errors.password_confirmation)" :error-message="form.errors.password_confirmation" v-model="form.password_confirmation" outlined label="Confirm Password" />
          </div>
        </q-card-section>
        <q-separator />
        <q-card-actions>
            <q-space />
          <q-btn
            unelevated
            color="primary"
            @click="updatePassword()"
            :loading="form.processing" icon="save"
            >Save</q-btn
          >
        </q-card-actions>
      </q-card>
    </q-form>

</template>
