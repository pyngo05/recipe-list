<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar class="bg-light-green">
        <q-toolbar-title> Yan's Recipes </q-toolbar-title>

        <q-space />
        <span class="q-pr-lg">
          <q-btn
            color="light-green-8"
            icon="add"
            label="Add"
            dense
            @click="alert = true"
          />
        </span>
        <q-btn
          color="light-green-8"
          icon="star"
          label="Favourites"
          dense
          @click="filterFavourites()"
        />
        <q-input
          dark
          dense
          outlined
          rounded
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
      <RecipeCard :filtered-favourites="filteredFavourites" />
      <!-- <router-view /> -->
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent } from "vue";
import RecipeCard from "components/RecipeCard.vue";

export default defineComponent({
  name: "MainLayout",

  components: {
    RecipeCard,
  },

  data() {
    return {
      searchTerm: null,
      alert: false,
      filteredFavourites: true,
    };
  },
  methods: {
    termSearched(term) {
      this.searchTerm = term;
    },
    showSearchResults() {
      //
    },
    filterFavourites() {
      this.filteredFavourites = !this.filteredFavourites;
      console.log("filteredFavourites", this.filteredFavourites);
    },
  },
});
</script>

<style lang="scss" scoped></style>
