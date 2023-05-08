<template>
  <q-dialog ref="dialog" @hide="onDialogHide" full-width full-height>
    <q-card class="q-dialog-plugin">
      <q-form @submit="onOKClick" class="row" ref="form">
        <q-input
          class="q-pa-md col-5"
          filled
          v-model="name"
          label="Recipe Name"
          lazy-rules
          dense
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-select
          class="q-pa-md offset-2 col-5"
          filled
          dense
          v-model="level"
          :options="options"
          label="Level"
          :rules="[(val) => val !== null || 'Please select level']"
        />

        <q-input
          class="q-pa-md col-3"
          dense
          filled
          v-model="servingSize"
          label="Serving Size"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-input
          class="q-pa-md col-3"
          filled
          v-model="prepTime"
          label="Prep Time"
          dense
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-input
          class="q-pa-md col-3"
          filled
          v-model="cookTime"
          label="Cook Time"
          lazy-rules
          dense
          :rules="[
            (val) =>
              (val && val.length > 0) ||
              'Please type something or enter \'None\'',
          ]"
        />

        <q-input
          class="q-pa-md col-3"
          filled
          v-model="totalTime"
          dense
          label="Total Time"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-input
          class="q-pa-md col-12"
          filled
          dense
          :model-value="ingredientEntered"
          label="Ingredients"
          lazy-rules
          @update:model-value="ingredientToAdd($event)"
          @keyup.enter="addToIngredients($event)"
          :rules="[
            () => this.ingredients.length > 0 || 'Please type something',
          ]"
        >
          <q-chip
            v-for="(ingredient, index) in ingredients"
            :key="index"
            dense
            removable
            :model-value="ingredient ? true : false"
            @remove="removeIngredient(ingredient)"
            color="primary"
            text-color="white"
          >
            {{ ingredient }}
          </q-chip>
        </q-input>

        <q-input
          class="q-pa-md col-12"
          filled
          v-model="image"
          dense
          label="Image URL"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-input
          class="q-pa-md col-12"
          filled
          dense
          v-model="description"
          label="Description"
          type="textarea"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <div class="q-pa-md col-12 text-subtitle1">Instructions:</div>
        <div
          class="col-12 row"
          v-for="(instruction, index) in instructions"
          :key="index"
        >
          <q-input
            class="q-pa-md col-12"
            filled
            dense
            v-model="instructions[index]"
            :label="`Step ${index + 1}:`"
            type="textarea"
            lazy-rules
            @update:model-value="instructionToAdd($event, index)"
            @keyup.enter="addToInstructions($event)"
            :rules="[
              (val) => (val && val.length > 0) || 'Please type something',
            ]"
          />
        </div>

        <div class="q-pa-md col-12">
          <q-btn
            dense
            color="primary"
            label="Add more steps"
            @click="addStep()"
          />
        </div>

        <q-toggle class="col-1" v-model="favourite" label="Add to favourites" />
      </q-form>
      <q-card-actions class="q-pt-xl" align="right">
        <q-btn color="primary" label="OK" @click="onOKClick" type="submit" />
        <q-btn color="primary" label="Cancel" @click="onCancelClick" />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script>
export default {
  name: "AddEditRecipeDialog",
  props: {
    recipe: Object,
  },
  emits: ["ok", "hide"],
  data() {
    return {
      name: null,
      description: null,
      instructions: ["", "", "", "", ""],
      favourite: false,
      level: null,
      image: null,
      options: ["Easy", "Medium", "Hard"],
      ingredients: [],
      servingSize: null,
      prepTime: null,
      cookTime: null,
      totalTime: null,
      ingredient: null,
      ingredientEntered: "",
      instruction: null,
      instructionEntered: "",
    };
  },
  mounted() {
    if (this.recipe.id) {
      console.log("recipe present", this.recipe);
    } else {
      console.log("recipe not present", this.recipe);
    }
  },
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
      this.$refs.form.validate().then((success) => {
        if (success) {
          this.$emit("ok", {
            name: this.name,
            description: this.description,
            instructions: this.instructions,
            favourite: this.favourite === false ? 0 : 1,
            level: this.level,
            image: this.image,
            ingredients: this.ingredients,
            servingSize: this.servingSize,
            prepTime: this.prepTime,
            cookTime: this.cookTime,
            totalTime: this.totalTime,
          });
          this.hide();
          this.$q.notify({
            color: "green-4",
            textColor: "white",
            icon: "cloud_done",
            message: "Recipe saved!",
          });
        } else {
          this.$q.notify({
            color: "negative",
            message: "Please fill in all fields",
          });
        }
      });
    },

    onCancelClick() {
      this.hide();
    },

    addStep() {
      this.instructions.push("");
    },

    removeIngredient(ingredient) {
      this.ingredients = this.ingredients.filter((item) => {
        return item !== ingredient;
      });
    },

    addToIngredients(ingredient) {
      this.ingredients.push(this.ingredient);
      this.ingredientEntered = "";
    },
    ingredientToAdd(ingredient) {
      this.ingredient = ingredient;
      this.ingredientEntered = ingredient;
    },
    instructionToAdd(instruction, index) {
      this.instruction = instruction;
      this.instructionEntered = instruction;
    },
    addToInstructions(instruction) {
      this.instructions.push(this.ingredient);
      // this.instructionEntered = "";
    },
  },
};
</script>
