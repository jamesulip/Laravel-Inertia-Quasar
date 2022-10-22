
  <script>
  import { ref } from 'vue'

  export default {
    setup () {
      const leftDrawerOpen = ref(false)

      return {
        leftDrawerOpen,
        toggleLeftDrawer () {
          leftDrawerOpen.value = !leftDrawerOpen.value
        }
      }
    }
  }
</script>


<template>
    <q-layout view="hHh lpR fFf">

      <q-header reveal bordered class="bg-primary text-white">
        <q-toolbar>
          <q-btn dense flat round icon="menu" @click="toggleLeftDrawer" />

          <q-toolbar-title>
            <q-avatar>
              <img src="https://cdn.quasar.dev/logo-v2/svg/logo-mono-white.svg">
            </q-avatar>
            Title
          </q-toolbar-title>
        </q-toolbar>
      </q-header>

      <q-drawer bordered v-model="leftDrawerOpen" side="left" overlay behavior="desktop">
        <!-- drawer content -->
      </q-drawer>

      <q-page-container>
        <slot/>
      </q-page-container>

    </q-layout>
  </template>
