<script setup>
import { ref } from "vue";
import { Inertia } from "@inertiajs/inertia";
import { Link, useForm } from "@inertiajs/inertia-vue3";
import ActionMessage from "@/Components/ActionMessage.vue";
import FormSection from "@/Components/FormSection.vue";
import InputError from "@/Components/InputError.vue";
import InputLabel from "@/Components/InputLabel.vue";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import SecondaryButton from "@/Components/SecondaryButton.vue";
import TextInput from "@/Components/TextInput.vue";

const props = defineProps({
  user: Object,
});

const form = useForm({
  _method: "PUT",
  name: props.user.name,
  email: props.user.email,
  photo: null,
});

const verificationLinkSent = ref(null);
const photoPreview = ref(null);
const photoInput = ref(null);

const updateProfileInformation = () => {
  if (photoInput.value) {
    form.photo = photoInput.value.files[0];
  }

  form.post(route("user-profile-information.update"), {
    errorBag: "updateProfileInformation",
    preserveScroll: true,
    onSuccess: () => clearPhotoFileInput(),
  });
};


const selectNewPhoto = () => {
  photoInput.value.click();
};

const updatePhotoPreview = () => {
  const photo = photoInput.value.files[0];

  if (!photo) return;

  const reader = new FileReader();

  reader.onload = (e) => {
    photoPreview.value = e.target.result;
  };

  reader.readAsDataURL(photo);
};

const deletePhoto = () => {
  Inertia.delete(route("current-user-photo.destroy"), {
    preserveScroll: true,
    onSuccess: () => {
      photoPreview.value = null;
      clearPhotoFileInput();
    },
  });
};

const clearPhotoFileInput = () => {
  if (photoInput.value?.value) {
    photoInput.value.value = null;
  }
};
</script>

<template>
    <q-form class="mx-auto ">
      <q-card flat bordered>
        <q-card-section>
          <div class="text-h6">Profile Information</div>
          <div class="text-subtitle2">
            Update your account's profile information and email address.
          </div>
        </q-card-section>
        <q-separator />
        <q-card-section>
          <div v-if="$page.props.jetstream.managesProfilePhotos">
            <input
              ref="photoInput"
              type="file"
              class="hidden"
              @change="updatePhotoPreview"
            />
            <div class="flex">
              <q-avatar v-show="!photoPreview" size="150px" class="mx-auto">
                <q-img :src="user.profile_photo_url" :alt="user.name" />
              </q-avatar>
              <q-avatar v-show="photoPreview" size="150px" class="mx-auto">
                <q-img :src="photoPreview" :alt="user.name" />
              </q-avatar>
            </div>
            <SecondaryButton
              class="mt-2 mr-2"
              type="button"
              @click.prevent="selectNewPhoto"
            >
              Select A New Photo
            </SecondaryButton>
            <SecondaryButton
              v-if="user.profile_photo_path"
              type="button"
              class="mt-2"
              @click.prevent="deletePhoto"
            >
              Remove Photo
            </SecondaryButton>
            <InputError :message="form.errors.photo" class="mt-2" />
          </div>
        </q-card-section>
        <q-card-actions>
          <div class="q-gutter-md w-full">
            <q-input type="text" v-model="form.name" outlined label="Name" />
            <q-input type="text" v-model="form.email" outlined label="Email" />
          </div>
        </q-card-actions>
        <q-separator />
        <q-card-actions>
          <q-space />
          <q-btn
            unelevated
            color="primary"
            @click="updateProfileInformation()"
            :loading="form.processing"
            >Save</q-btn
          >
        </q-card-actions>
      </q-card>
    </q-form>
</template>
