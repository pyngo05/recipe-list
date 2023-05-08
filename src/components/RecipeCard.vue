<template>
  <q-card
    class="recipe-card cursor-pointer"
    flat
    bordered
    @click="openRecipeInstructionsDialog(recipe)"
  >
    <q-img class="img" :src="recipe.image">
      <q-rating
        class="rating"
        :model-value="recipe.favourite"
        max="1"
        size="sm"
        color="yellow"
        icon="star_border"
        icon-selected="star"
        no-dimming
        @click.stop
        @update:model-value="emitUpdateFavourites(recipe, $event)"
      ></q-rating>
    </q-img>

    <q-card-section>
      <div
        class="text-overline"
        :class="
          recipe.level === 'Easy'
            ? 'text-positive'
            : recipe.level === 'Medium'
            ? 'text-warning'
            : 'text-negative'
        "
      >
        {{ recipe.level }}
      </div>
      <div class="text-h5 q-mt-sm q-mb-xs ellipsis">{{ recipe.name }}</div>
      <div class="text-caption text-grey ellipsis-3-lines">
        {{ recipe.description }}
      </div>
    </q-card-section>
  </q-card>
</template>

<script>
import { defineComponent } from "vue";
import RecipeInstructionsDialog from "components/RecipeInstructionsDialog.vue";

export default defineComponent({
  name: "RecipeCard",
  emits: ["favouriteUpdated", "deleteRecipe", "editRecipe"],
  props: {
    recipe: Object,
  },
  methods: {
    emitUpdateFavourites(recipe, rating) {
      this.$emit("favouriteUpdated", { recipe, rating });
    },

    openRecipeInstructionsDialog(recipe) {
      this.$q
        .dialog({
          component: RecipeInstructionsDialog,
          componentProps: { recipe },
        })
        .onOk((data) => {
          if (data?.recipeToDelete)
            this.$emit("deleteRecipe", data.recipeToDelete);
          if (data?.recipeToEdit) this.$emit("editRecipe", data.recipeToEdit);
        });
    },
  },
});
</script>

<style lang="scss" scoped>
.recipe-card {
  width: 100%;
  max-width: 250px;
  min-height: 470px;
}
.rating {
  background-color: transparent;
}
.img {
  width: 248px;
  height: 300px;
  object-fit: cover;
}
</style>
