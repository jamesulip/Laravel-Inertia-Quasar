<script setup>
import { ref } from "vue";
import { useForm } from "@inertiajs/inertia-vue3";
import ActionSection from "@/Components/ActionSection.vue";
import DangerButton from "@/Components/DangerButton.vue";
import DialogModal from "@/Components/DialogModal.vue";
import InputError from "@/Components/InputError.vue";
import SecondaryButton from "@/Components/SecondaryButton.vue";
import TextInput from "@/Components/TextInput.vue";

const confirmingUserDeletion = ref(false);
const passwordInput = ref(null);

const form = useForm({
  password: "",
});

const confirmUserDeletion = () => {
  confirmingUserDeletion.value = true;

  setTimeout(() => passwordInput.value.focus(), 250);
};

const deleteUser = () => {
  form.delete(route("current-user.destroy"), {
    preserveScroll: true,
    onSuccess: () => closeModal(),
    onError: () => passwordInput.value.focus(),
    onFinish: () => form.reset(),
  });
};

const closeModal = () => {
  confirmingUserDeletion.value = false;

  form.reset();
};
const confirmDeleteModal = ref(false);
</script>

<template>
    <q-dialog v-model="confirmDeleteModal">
      <q-card>
        <q-card-section>
          <div class="text-h6">Delete Account</div>
          <div class="text-subtitle2 mb-3">
            Are you sure you want to delete your account? Once your account is deleted,
            all of its resources and data will be permanently deleted. Please enter your
            password to confirm you would like to permanently delete your account.
          </div>
          <q-input    ref="passwordInput" dense type="password" :error="Boolean(form.errors.password)" :error-message="form.errors.password" v-model="form.password" outlined label="New Password" />

        </q-card-section>
        <q-separator/>
        <q-card-actions>
            <q-space/>
            <q-btn unelevated label="Cancel"/>
            <q-btn unelevated :loading="form.processing" color="negative" @click="deleteUser()" label="Delete Account"/>
        </q-card-actions>
      </q-card>
    </q-dialog>
    <q-form class="mx-auto">
      <q-card flat bordered>
        <q-card-section>
          <div class="text-h6">Delete Account</div>
          <div class="text-subtitle2">Dermanently delete your account.</div>
        </q-card-section>
        <q-separator />
        <q-card-section>
          <div class="max-w-xl text-sm text-gray-600">
            Once your account is deleted, all of its resources and data will be
            permanently deleted. Before deleting your account, please download any data or
            information that you wish to retain.
          </div>

        </q-card-section>
        <q-separator/>
        <q-card-actions>
        <q-space/>
            <q-btn icon="delete" @click="confirmDeleteModal = true" color="negative" unelevated=""
            >Delete Account</q-btn
          ></q-card-actions>
      </q-card>
    </q-form>

</template>
