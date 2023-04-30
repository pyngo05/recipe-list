<template>
  <div class="q-pa-md row items-start q-gutter-md">
    <div v-for="recipe in filteredRecipes" :key="recipe.id">
      <recipe-card
        :recipe="recipe"
        @favouriteUpdated="updateFavourites"
      ></recipe-card>
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";
import RecipeCard from "./RecipeCard.vue";

export default defineComponent({
  name: "RecipeCards",
  components: { RecipeCard },
  props: {
    filteredFavourites: Boolean,
    searchFiltered: Boolean,
    searchTerm: String,
  },
  data() {
    return {
      // a favourite rating of 0 is false, 1 is true
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
  computed: {
    filteredRecipes() {
      return this.recipes.filter((recipe) => {
        // exclude non favourites
        if (this.filteredFavourites && recipe.favourite === 0) {
          return false;
        }

        // exclude non-matches for search term
        if (
          this.searchTerm &&
          !recipe.name.toLowerCase().includes(this.searchTerm.toLowerCase())
        ) {
          return false;
        }

        return true;
      });
    },
  },
  methods: {
    updateFavourites(data) {
      if (this.recipes[data.recipe.id - 1].favourite === 1) {
        this.recipes[data.recipe.id - 1].favourite = 0;
      } else {
        this.recipes[data.recipe.id - 1].favourite = 1;
      }
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
