<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar class="bg-light-green">
        <q-toolbar-title class="noEllipsis"> Yan's Recipes </q-toolbar-title>

        <q-space />
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
      <div class="row q-pa-lg justify-between">
        <q-btn
          color="light-green-8"
          icon="add"
          label="Add Recipe"
          dense
          @click="openAddEditRecipeDialog({})"
        />
        <q-toggle
          color="light-green-8"
          icon="star"
          label="Favourites"
          :model-value="filteredFavourites"
          @click="filterFavourites()"
        />
      </div>
      <RecipeCards
        :filtered-favourites="filteredFavourites"
        :search-filtered="searchFiltered"
        :search-term="searchTerm"
        :new-recipe="newRecipe"
        :edited-recipe="editedRecipe"
        @edit-recipe="editRecipe"
      />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent } from "vue";
import RecipeCards from "components/RecipeCards.vue";
import AddEditRecipeDialog from "components/AddEditRecipeDialog.vue";

export default defineComponent({
  name: "MainLayout",

  components: {
    RecipeCards,
  },

  data() {
    return {
      searchTerm: "",
      filteredFavourites: false,
      searchFiltered: false,
      newRecipe: {},
      editedRecipe: {},
    };
  },
  methods: {
    termSearched(term) {
      this.searchTerm = term;
    },

    filterFavourites() {
      this.filteredFavourites = !this.filteredFavourites;
    },

    openAddEditRecipeDialog(recipe) {
      this.$q
        .dialog({
          component: AddEditRecipeDialog,
          componentProps: { recipe },
        })
        .onOk((data) => {
          data.newRecipe && data.newRecipe === true
            ? (this.newRecipe = data)
            : (this.editedRecipe = data);
        });
    },
    editRecipe(recipe) {
      this.openAddEditRecipeDialog(recipe);
    },
  },
});
</script>

<style lang="scss" scoped>
.noEllipsis {
  overflow: visible;
}
</style>
