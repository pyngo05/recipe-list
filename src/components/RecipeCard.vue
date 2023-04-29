<template>
  <div class="q-pa-md row items-start q-gutter-md">
    <q-card
      class="my-card cursor-pointer"
      flat
      bordered
      v-for="recipe in recipes"
      :key="recipe.id"
    >
      <q-img :src="recipe.image">
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

      <q-card-section>
        <div class="text-overline text-orange-9">{{ recipe.level }}</div>
        <div class="text-h5 q-mt-sm q-mb-xs">{{ recipe.name }}</div>
        <div class="text-caption text-grey">
          {{ recipe.description }}
        </div>
      </q-card-section>
    </q-card>
  </div>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "RecipeCard",
  props: {
    filteredFavourites: Boolean,
  },
  watch: {
    filteredFavourites() {
      console.log("filteredFavourites changed", this.filteredFavourites);
    },
  },
  data() {
    return {
      recipes: [
        {
          id: 1,
          name: "Pao de Queijo",
          level: "Easy",
          description:
            "These small baked cheese rolls are a popular snack and breakfast food in Brazil.",
          image:
            "https://images.unsplash.com/photo-1598142982901-df6cec10ae35?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=870&q=80",
          favourite: 0,
        },
        {
          id: 2,
          name: "Kimchi Kimbap",
          level: "Medium",
          description:
            "A seaweed rice roll filled with a variety of delicious fillings, which features kimchi as the main filling.",
          image:
            "https://images.unsplash.com/photo-1643887259964-866f4b1076d6?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=387&q=80",
          favourite: 0,
        },
        {
          id: 3,
          name: "Brigadeiro",
          level: "Easy",
          description:
            "Brigadeiro is a Brazilian chocolate truffle garnished with sprinkles.",
          image:
            "https://images.unsplash.com/photo-1630953899906-d16511a72558?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1615&q=80",
          favourite: 0,
        },
        {
          id: 4,
          name: "Bibimbap",
          level: "Medium",
          description:
            "A bowl of rice topped with an array of individually prepared vegetables and meat.",
          image:
            "https://images.unsplash.com/photo-1639321905636-c1c0e71fb467?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1548&q=80",
          favourite: 0,
        },
        {
          id: 5,
          name: "Moon Cake",
          level: "Hard",
          description:
            "Baked pastries consisting of a rich, sweet filling and a thin, moist, golden brown crust.",
          image:
            "https://images.unsplash.com/photo-1630859288268-d11728472a25?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=435&q=80",
          favourite: 1,
        },
      ],
    };
  },
  methods: {
    updateFavourites(recipe, rating) {
      console.log("rating", rating);
      if (this.recipes[recipe.id - 1].favourite === 1) {
        this.recipes[recipe.id - 1].favourite = 0;
        console.log("this.recipes[recipe.id - 1]", this.recipes[recipe.id - 1]);
      } else {
        this.recipes[recipe.id - 1].favourite = 1;
        console.log("this.recipes[recipe.id - 1]", this.recipes[recipe.id - 1]);
      }
      console.log("this.recipes", this.recipes);
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
</style>
