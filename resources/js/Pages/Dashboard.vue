<script setup>
import AppLayout from '@/Layouts/AppLayout.vue';
import Welcome from '@/Components/Welcome.vue';
</script>

<template>
    <AppLayout title="Dashboard">
        <q-page padding>
            <Welcome />
        </q-page>
    </AppLayout>
</template>
