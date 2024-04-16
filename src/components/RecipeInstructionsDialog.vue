<template>
  <q-dialog ref="dialog" @hide="onDialogHide" full-width>
    <q-card class="recipe-card q-pb-xl">
      <q-card-actions align="right">
        <q-btn flat round dense icon="close" @click="onOKClick" />
      </q-card-actions>
      <q-card-actions align="center">
        <q-btn
          color="primary"
          label="Edit"
          icon="edit"
          @click="onEditRecipeClick(recipe)"
        />
        <q-btn
          color="primary"
          label="Delete"
          @click="onDeleteRecipeClick(recipe)"
        />
      </q-card-actions>

      <div class="column items-center q-pa-lg">
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
            @update:model-value="updateFavourites(recipe, $event)"
          ></q-rating>
        </q-img>

        <div class="text-h5 q-mt-sm q-mb-xs">
          {{ recipe.name }}
          <q-chip
            dense
            :color="
              recipe.level === 'Easy'
                ? 'positive'
                : recipe.level === 'Medium'
                ? 'warning'
                : 'negative'
            "
            text-color="white"
          >
            {{ recipe.level }}
          </q-chip>
        </div>

        <div class="text-caption text-grey description">
          {{ recipe.description }}
        </div>

        <div class="q-pt-md q-pb-md">
          <q-chip dense outline color="primary" icon="restaurant">
            {{ "Serves: " + recipe.servingSize }}
          </q-chip>
          <q-chip dense outline color="primary">
            {{ "Prep time: " + recipe.prepTime }}
          </q-chip>
          <q-chip dense outline color="primary">
            {{ "Cook time: " + recipe.cookTime }}
          </q-chip>
          <q-chip dense outline color="primary" icon="timer">
            {{ "Total time: " + recipe.totalTime }}
          </q-chip>
        </div>

        <div class="q-pt-md">
          <span class="q-pr-xl">
            {{ "Ingredients:" }}
            <ul>
              <li
                v-for="(ingredient, index) in recipe.ingredients"
                :key="index"
              >
                {{ ingredient }}
              </li>
            </ul>
          </span>

          <span class="q-pl-xl">
            {{ "Instructions:" }}
            <ol>
              <li
                class="q-pb-sm"
                v-for="(instruction, index) in recipe.instructions"
                :key="index"
              >
                {{ instruction }}
              </li>
            </ol>
          </span>
        </div>
      </div>
    </q-card>
  </q-dialog>
</template>

<script>
export default {
  name: "RecipeInstructionsDialog",
  props: {
    recipe: Object,
  },
  data() {
    return {
      favouritesUpdated: false,
      update: null,
    };
  },
  emits: ["ok", "hide", "delete"],
  methods: {
    show() {
      this.$refs.dialog.show();
    },

    hide() {
      this.$refs.dialog.hide();
    },

    onDialogHide() {
      this.$emit("hide");
    },

    onOKClick() {
      if (this.favouritesUpdated === true) {
        this.$emit("ok", {
          recipe: this.update.recipe,
          rating: this.update.rating,
        });
      } else {
        this.$emit("ok");
      }
      this.favouritesUpdated = false;
      this.hide();
    },

    updateFavourites(recipe, rating) {
      this.favouritesUpdated = true;
      this.update = { recipe, rating };
      recipe.favourite === 1 ? (recipe.favourite = 0) : (recipe.favourite = 1);
    },

    onDeleteRecipeClick(recipe) {
      this.$q
        .dialog({
          title: "Delete Recipe",
          message: `Are you sure you would like to delete ${recipe.name}?`,
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.$emit("ok", { recipeToDelete: recipe });
          this.hide();
        })
        .onCancel(() => {
          this.hide();
        });
    },

    onEditRecipeClick(recipe) {
      this.$emit("ok", { recipeToEdit: recipe });
    },
  },
};
</script>

<style lang="scss" scoped>
.recipe-card {
  width: 100%;
  max-width: 100%;
  min-height: 100vh;
}

.img {
  width: 100%;
  height: auto;
  object-fit: cover;
}

.description {
  font-size: 14px;
}
</style>
