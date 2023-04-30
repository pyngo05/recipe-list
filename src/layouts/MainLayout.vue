<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar class="bg-light-green">
        <q-toolbar-title> Yan's Recipes </q-toolbar-title>

        <q-space />
        <q-toggle
          color="light-green-8"
          icon="star"
          label="Favourites"
          :model-value="filteredFavourites"
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
      <div class="row q-pa-lg">
        <q-btn
          class="offset-11 col-1"
          color="light-green-8"
          icon="add"
          label="Add Recipe"
          dense
          @click="alert = true"
        />
      </div>
      <RecipeCards
        :filtered-favourites="filteredFavourites"
        :search-filtered="searchFiltered"
        :search-term="searchTerm"
      />
      <!-- <router-view /> -->
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent } from "vue";
import RecipeCards from "components/RecipeCards.vue";

export default defineComponent({
  name: "MainLayout",

  components: {
    RecipeCards,
  },

  data() {
    return {
      searchTerm: "",
      alert: false,
      filteredFavourites: false,
      searchFiltered: false,
    };
  },
  methods: {
    termSearched(term) {
      this.searchTerm = term;
    },
    filterFavourites() {
      this.filteredFavourites = !this.filteredFavourites;
    },
  },
});
</script>

<style lang="scss" scoped></style>
