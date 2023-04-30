<template>
  <q-card class="my-card cursor-pointer" flat bordered>
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
      <div class="text-h5 q-mt-sm q-mb-xs">{{ recipe.name }}</div>
      <div class="text-caption text-grey">
        {{ recipe.description }}
      </div>
    </q-card-section>
  </q-card>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "RecipeCard",
  emits: ["favouriteUpdated"],
  props: {
    recipe: Object,
  },
  methods: {
    emitUpdateFavourites(recipe, rating) {
      this.$emit("favouriteUpdated", { recipe, rating });
    },
  },
});
</script>

<style lang="scss" scoped>
.my-card {
  width: 100%;
  max-width: 250px;
}
.rating {
  background-color: transparent;
}
.img {
  border-radius: 50%;
}
</style>
