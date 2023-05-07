<template>
  <div class="q-pl-xl row items-start q-gutter-md justify-start">
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
    searchTerm: String,
    newRecipe: Object,
  },
  watch: {
    newRecipe() {
      if (this.newRecipe !== null) this.recipes.push(this.newRecipe);
      this.recipes[this.recipes.length - 1].id = this.recipes.length;
    },
  },
  data() {
    return {
      // a favourite rating of 0 is false, 1 is true
      recipes: [
        {
          id: 1,
          name: "Pão de Queijo",
          level: "Easy",
          description:
            "These small baked cheese rolls are a popular snack and breakfast food in Brazil.",
          image:
            "https://images.unsplash.com/photo-1598142982901-df6cec10ae35?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=870&q=80",
          favourite: 0,
          servingSize: "30 Cheese Rolls",
          prepTime: "10 mins",
          cookTime: "20 mins",
          totalTime: "30 mins",
          instructions: [
            "Preheat oven to 400F degrees with a rack in the middle.",
            "Combine the milk, water, oil and salt in a saucepan and bring to a boil over medium high heat.",
            "Add the tapioca flour to the bowl of a stand mixer and, once the milk mixture boils, pour it over the flour. Turn the mixer on and mix it well. The texture will be fondant-like, really white and sticky.",
            "With the mixer still on, add the eggs, one at a time. You will think they won't mix, since the tapioca flour mixture is so sticky, but hang in there cause they will.",
            "Once the eggs are incorporated, add the cheese, a little at a time, until fully incorporated.",
            "The dough is supposed to be soft and sticky. However, if you're worried it's too liquidy, add some more tapioca flour. Just don't over do it or your cheese bread will be tough and not too gooey.",
            "To shape the balls, wet your hands with cold water and, using a spoon, scoop some of the dough to shape balls that are a little smaller than golf-sized.",
            "Place the balls on a baking sheet covered with parchment paper and bring it to the preheat oven.",
            "Bake for 15-20 minutes or until they are golden and puffed.",
          ],
          ingredients: [
            "4 cups tapioca flour",
            "1 1/4 cups milk",
            "1/2 cup water",
            "6 tablespoons oil",
            "1 1/2 cups grated parmesan cheese",
            "1 cups shredded Mozzarella cheese",
            "2 large eggs",
            "2 teaspoons salt",
          ],
        },
        {
          id: 2,
          name: "Raspberry, Pistachio and White Chocolate Cake",
          level: "Medium",
          description: "This cake is sweet, salty, tangy and super light.",
          image:
            "https://images.unsplash.com/photo-1565958011703-44f9829ba187?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1530&q=80",
          favourite: 0,
          servingSize: "10 Slices",
          prepTime: "2 hrs",
          cookTime: "35 mins",
          totalTime: "3 hrs 15 mins",
          instructions: [
            "For the sponge: Preheat the oven to 180C/160C Fan/Gas Mark 4. Then grease and line two 8x10in cake tins and set aside. In a large mixing bowl, using an electric hand whisk cream together the butter and sugar for 3-5 minutes until very light and fluffy.",
            "Whisk in the vanilla and eggs until just combined. Then whisk in the flour, ground pistachios and baking powder until just combined. Finally, add the milk (and a small blob of green food colouring if desired) and whisk again until just combined.",
            "Divide the batter between the two prepared cake tins, and bake for 25 minutes until golden and a toothpick inserted into the centre comes out clean. Leave for 10 minutes in the tin and then remove and place onto a cooling rack to cool completely.",
            "For the buttercream: Place the egg whites and sugar into the bowl of a stand mixer and cook over a bain-marie. Whisk intermittently until the mixture reaches 70C on a food thermometer. Then take off the heat and fit to a stand mixer with the whisk attachment. Whisk on a high speed for around 10 minutes until you have a glossy and thick meringue, that has cooled to room temperature.",
            "Then swap out the whisk attachment for the paddle attachment, and with the mixer running on a medium speed, add the butter one tablespoon at a time. Making sure each addition is fully incorporated before adding the next. Once all the butter has been encoporated, mix on a high speed for a few minutes more.",
            "Finally, melt the white chocolate and mix into the buttercream along with the vanilla.",
            "To assemble: Place all but 130g of the buttercream into a piping bag and cut off the end to create a medium sized hole. Colour 70g of the remaining buttercream with a little green food colouring, then place into a piping bag and cut off the end to create a small hole. Finally, place the remaining 30g buttercream into a piping bag and cut off the end to create a small hole. Then halve your raspberries in half and set everything aside.",
            "Trim the edges of your sponges so they are all the same size, then cut each sponge in half widthways (so you end up with four sponges approx. 4x5in). Then place one sponge onto a cake board or serving platter and pipe a little buttercream on top, smoothing with a palette knife or spoon. Then place 1/3 of the halved raspberries on top, pipe a few blobs of buttercream on top of this to help stick. Then place your next sponge layer on top and press down gently. Repeat this process for the next two sponge layers, until you are left with the final sponge layer to place on top. Spread a thin layer of the buttercream over the top and sides of the cake, using a palette knife or cake scraper to get this even. Then place in the fridge for 30 minutes to set.",
            "Once the crumb coat is chilled, remove from the fridge and spread the remaining buttercream over the top and sides of the cake. Again, using a palette knife or cake scraper to keep this even. Finally, using the green buttercream pipe small oval shapes to create 'easter eggs' all over the cake. Finish with the white buttercream to create zigzag lines on the easter eggs.",
          ],
          ingredients: [
            "225g unsalted butter room temperature",
            "225g caster sugar",
            "1 tsp vanilla extract",
            "2 large eggs",
            "175g plain flour",
            "75g ground pistachios",
            "2 tsp baking powder",
            "75ml milk whole or semi-skimmed",
            "green food colouring (optional)",
            "150ml egg whites, approx. 3 large eggs",
            "250g caster sugar",
            "450g unsalted butter room temperature",
            "2 tsp vanilla extract",
            "150 g white chocolate",
            "green food colouring (optional)",
            "1 punnet of raspberries",
          ],
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
          servingSize: "20 Truffles",
          prepTime: "15 mins",
          cookTime: "10 mins",
          totalTime: "45 mins",
          instructions: [
            "Grease a plate with butter and set aside.",
            "In a medium non-stick pan, combine the condensed milk, butter, and cocoa powder in medium heat and mix it well.",
            "The mixture thickens after about 10-12 minutes, and once you can run your silicone spatula through the middle of it without it running back together for 2-3 seconds, it is done.",
            "Pour the mixture onto the lightly greased plate and let it chill in the fridge for an hour.",
            "In the meantime, prepare the candy cups and place the sprinkles in a bowl.",
            "Once chilled, you are ready to roll the little balls. Lightly wet your hands, put a tablespoon of the chewy mixture on your hands, and roll into a small ball.",
            "Roll it over the sprinkles.",
            "Place the balls in the candy cups",
          ],
          ingredients: [
            "397g can sweetened condensed milk",
            "3 tbsp unsweetened cocoa powder, sifted",
            "1 tbsp butter",
            "A pinch of salt to enhance the flavour",
            "Chocolate sprinkles",
          ],
        },
        {
          id: 4,
          name: "French Toast",
          level: "Easy",
          description:
            "This classic breakfast dish has plenty of options for toppings.",
          image:
            "https://images.unsplash.com/photo-1484723091739-30a097e8f929?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=449&q=80",
          favourite: 0,
          servingSize: "4",
          prepTime: "5 mins",
          cookTime: "15 mins",
          totalTime: "20 mins",
          instructions: [
            "Whisk together the eggs, milk, cream, vanilla and cinnamon. Lay the brioche slices in a single layer in a shallow dish and pour the egg mixture over them. Allow to soak for 2-3 mins, then carefully turn over and soak for 2 mins more.",
            "Heat 1 tbsp of the vegetable oil and butter in a non-stick frying pan over a medium heat until foaming. Carefully lift 2 slices of the soaked brioche out of the dish and add to the frying pan. Fry for 3 mins on each side, until golden and crisp, then place on a wire rack over a baking tray in a warm oven while you repeat with the remaining slices.",
            "Serve dusted in icing sugar and scattered with fresh berries, if you like.",
          ],
          ingredients: [
            "2 large eggs",
            "80ml whole milk",
            "40ml double cream",
            "1 tsp vanilla extract",
            "1/2 tsp cinnamon",
            "4 thick slices brioche",
            "2 tbsp vegetable oil",
            "2 tbsp butter",
            "icing sugar and fresh fruit, to serve (optional)",
          ],
        },
        {
          id: 5,
          name: "Summer Rolls",
          level: "Easy",
          description:
            "Fresh-tasting rice paper rolls packed with shellfish, light vermicelli noodles, carrots, cucumber and herbs, with a sweet ginger chilli dipping sauce.",
          image:
            "https://images.unsplash.com/photo-1594020293008-5f99f60bd4d7?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=906&q=80",
          favourite: 1,
          servingSize: "12 Rolls",
          prepTime: "30 mins",
          cookTime: "None",
          totalTime: "30 mins",
          instructions: [
            "Make the dipping sauce by pounding the garlic, chilli, ginger and sugar together using a pestle and mortar, then stir in the fish sauce and lime juice. Alternatively, just blitz everything together in a mini blender.",
            "Soak the rice noodles in a bowl of hot water for 15 mins, then drain well. Assemble all the prepared filling ingredients – once you start, you will need everything to hand.",
            "When you are ready to make the rolls, dip one of the rice papers in a bowl of hot water, moving it around until the whole wrapper is soft – about 10-15 secs – then drain on a tea towel.",
            "Place a rice paper wrapper on a board and at one edge of the wrapper, add a few mint leaves, then three prawn halves.",
            "Place some lettuce on top of the prawns, followed by some noodles, a few strips of carrot and cucumber, some more herbs and finally some beansprouts. Don’t overfill the pancakes or they will be hard to roll.",
            "Lift the edge of the rice paper wrapper nearest to you over the filling and, holding the filling in position with your fingers, start rolling up tightly.",
            "When you’re about halfway, fold the ends of the rice paper in and over the filling so that it is completely enclosed.",
            "Keep on rolling tightly until the whole rice paper wrapper is rolled up. To serve, cut the rolls in half on the diagonal.",
          ],
          ingredients: [
            "2 garlic cloves, finely chopped",
            "1 small red chilli (deseeded if you don't like it too hot), finely chopped",
            "1 tbsp finely chopped ginger",
            "1 tbsp golden caster sugar",
            "3 tbsp fish sauce",
            "juice 1 lime",
            "100g vermicelli rice noodle or bean thread noodles",
            "12 x 20cm round rice paper wrappers",
            "handful mint leaves",
            "18 cooked prawns, cut in half lengthways",
            "2-3 large iceberg lettuce leaves, torn into 12 pieces",
            "1 carrot, cut into thin batons",
            "a handful coriander",
            "a handful Thai basil",
            "a handful long chives",
            "50g beansprout",
          ],
        },
        {
          id: 6,
          name: "Beef and Chinese Chive Dumplings",
          level: "Hard",
          description:
            "The filling is simple, consisting of only beef and Chinese chives.",
          image:
            "https://images.unsplash.com/photo-1496116218417-1a781b1c416c?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=870&q=80",
          favourite: 1,
          servingSize: "30 Dumplings",
          prepTime: "50 mins",
          cookTime: "15 mins",
          totalTime: "1 hr 5 mins",
          instructions: [
            "In a mixing bowl, add water to flour gradually. Mix with chopsticks/spatula until no more loose flour can be seen. Combine and knead into a dough.",
            "Leave to rest (covered) for 10 minutes then knead again until smooth.",
            "Cover and rest the dough again for 30-60 minutes until it becomes soft.",
            "While waiting for the dough to rest, mix the filling of your choice.",
            "Divide the dough into three parts. Roll one part into a rope then cut into 10 equal sections (cover the rest to avoid drying out).",
            "Press each piece into a small disc with the palm of your hand.",
            "Use a rolling pin to flatten it into a thin disc (Please refer to the tutorial video below). Dust with flour if it sticks. Repeat to finished the rest of the dough.",
            "Place a spoonful of filling on the wrapper. Seal the wrapper using the techniqe you're most comfortable with.",
            "Heat up a little oil over high heat. Place in dumplings (Gently move the pan back and forth to make sure each dumpling is in touch with the oil).",
            "When the bottom part of the dumplings becomes golden brown, pour in cold water (enough to cover ⅓ of the dumplings). Cover with a lid (remain high heat).",
            "Uncover when the water evaporates completely. Cook another 30 seconds or so to crisp up.",
            "Serve the dumplings warm with a mix of chili oil and black rice vinegar.",
          ],
          ingredients: [
            "250 g all-purpose flour/plain flour - about 2 cups",
            "130 g water - 1/2 cup+2 teaspoon",
            "Beef & Chinese chive filling",
            "neutral cooking oil",
            "Black rice vinegar",
            "Chili oil",
          ],
        },
        {
          id: 7,
          name: "Açaí Bowl",
          level: "Easy",
          description:
            "Made with superfruit acai berry puree, frozen bananas, blueberries and strawberries then topped with fresh fruit, granola, and seeds.",
          image:
            "https://images.unsplash.com/photo-1627308594190-a057cd4bfac8?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=436&q=80",
          favourite: 1,
          servingSize: "1",
          prepTime: "5 mins",
          cookTime: "None",
          totalTime: "5 mins",
          instructions: [
            "Put the açaí powder, frozen berries, banana and ice cubes into a powerful blender with 100ml water. Blitz until smooth, then pour into a bowl and add your choice of toppings.",
          ],
          ingredients: [
            "2 tsp açaí powder",
            "handful frozen berries",
            "1/2 a very ripe banana, chopped",
            "handful ice cubes",
            "1 tsp coconut flakes, 5 pineapple chunks, ½ passionfruit, 1 tbsp toasted oats, to top (optional)",
          ],
        },
        {
          id: 8,
          name: "Coconut Mango Chia Pudding",
          level: "Easy",
          description:
            "Thick and creamy mango chia pudding with layers of coconut milk chia pudding and mango puree.",
          image:
            "https://images.unsplash.com/photo-1629180050285-7c56c6671f19?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=385&q=80",
          favourite: 0,
          servingSize: "2",
          prepTime: "15 mins",
          cookTime: "None",
          totalTime: "15 mins",
          instructions: [
            "Stir together coconut milk, chia seeds and maple syrup. Let sit for 5 minutes then stir the mixture again to get out any clumps that formed. Place mixture in the fridge to set, at least 30 minutes but up to 12 hours.",
            "While chia pudding is setting, peel mango and remove flesh. Place mango flesh in a high powdered blender and blend until smooth.",
            "Once chia pudding has set, grab two jars and spoon chia pudding into each. Top with a layer of mango puree and then continue to layer. I did two layers and ended with the chia pudding. Sprinkle a few pieces of toasted coconut on top and enjoy.",
            "Refrigerate any leftovers. Mango chia pudding should keep for at least 3-4 days.",
          ],
          ingredients: [
            "1 cup light coconut milk, from a can",
            "3 tbsp chia seeds",
            "1 tbsp maple syrup, or sweetener of choice",
            "1 small mango, peeled",
            "toasted coconut flakes, for topping",
          ],
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
