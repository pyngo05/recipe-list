<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar class="bg-light-green">
        <q-toolbar-title> Yan's Recipes </q-toolbar-title>

        <q-space />

        <q-input
          dark
          dense
          standout
          v-model="searchTerm"
          input-class="text-right"
          class="q-ml-md"
          label="Search..."
          @update:model-value="termSearched($event)"
          @keyup.enter="showSearchResults"
        >
          <template v-slot:append>
            <q-icon v-if="searchTerm === ''" name="search" />
            <q-icon
              v-else
              name="clear"
              class="cursor-pointer"
              @click="searchTerm = ''"
            />
          </template>
        </q-input>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "MainLayout",

  components: {},

  setup() {
    return {
      searchTerm: null,
    };
  },
  methods: {
    termSearched(term) {
      this.searchTerm = term;
    },
    showSearchResults() {
      //
    },
  },
});
</script>
