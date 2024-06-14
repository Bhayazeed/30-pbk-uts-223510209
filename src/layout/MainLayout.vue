<template>
    <q-layout view="lHr lpR lfr">
      <q-header class="bg-primary text-white">
        <q-toolbar>
          <q-toolbar-title>M. Bhayaziid Amalin PBK</q-toolbar-title>
          <q-tabs align="left">
            <q-route-tab to="/" label="Todo List" />
            <q-route-tab to="/posts" label="Post" />
            <q-route-tab to="/albums" label="Album" />
          </q-tabs>
        </q-toolbar>
      </q-header>
      <q-page-container style="padding: 0;">
        <router-view />
      </q-page-container>
      <q-footer class="bg-primary text-white" height-hint="98">
        <q-toolbar>
          <q-toolbar-title align="left">
            Copyright © M. Bhayaziid Amalin®. All rights reserved.
          </q-toolbar-title>
        </q-toolbar>
      </q-footer>
    </q-layout>
</template>
  
<script setup>
</script>
  
<style>
  .q-toolbar {
    padding: 0 75px !important;
  }
  .q-tab {
    padding: 0 20px !important;
    min-height: 75px !important;
  }
</style>