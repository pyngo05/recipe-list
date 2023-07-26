<template>
  <q-dialog ref="dialog" @hide="onDialogHide" full-width full-height>
    <q-card class="q-dialog-plugin">
      <div
        v-if="!editedRecipe.name"
        class="text-h4 q-pa-lg text-center text-primary text-bold"
      >
        Add Recipe
      </div>
      <div v-else class="text-h4 text-center q-pa-lg text-primary text-bold">
        Edit Recipe
      </div>
      <q-form @submit="onOKClick" class="row" ref="form">
        <q-input
          class="q-pa-md col-5"
          filled
          :model-value="editedRecipe.name ? editedRecipe.name : name"
          label="Recipe Name"
          lazy-rules
          dense
          @update:model-value="
            editedRecipe.name ? (editedRecipe.name = $event) : (name = $event)
          "
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-select
          class="q-pa-md offset-2 col-5"
          filled
          dense
          :model-value="editedRecipe.level ? editedRecipe.level : level"
          @update:model-value="
            editedRecipe.level
              ? (editedRecipe.level = $event)
              : (level = $event)
          "
          :options="options"
          label="Level"
          :rules="[(val) => val !== null || 'Please select level']"
        />

        <q-input
          class="q-pa-md col-3"
          dense
          filled
          :model-value="
            editedRecipe.servingSize ? editedRecipe.servingSize : servingSize
          "
          @update:model-value="
            editedRecipe.servingSize
              ? (editedRecipe.servingSize = $event)
              : (servingSize = $event)
          "
          label="Serving Size"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-input
          class="q-pa-md col-3"
          filled
          :model-value="
            editedRecipe.prepTime ? editedRecipe.prepTime : prepTime
          "
          @update:model-value="
            editedRecipe.prepTime
              ? (editedRecipe.prepTime = $event)
              : (prepTime = $event)
          "
          label="Prep Time"
          dense
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-input
          class="q-pa-md col-3"
          filled
          :model-value="
            editedRecipe.cookTime ? editedRecipe.cookTime : cookTime
          "
          @update:model-value="
            editedRecipe.cookTime
              ? (editedRecipe.cookTime = $event)
              : (cookTime = $event)
          "
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
          :model-value="
            editedRecipe.totalTime ? editedRecipe.totalTime : totalTime
          "
          @update:model-value="
            editedRecipe.totalTime
              ? (editedRecipe.totalTime = $event)
              : (totalTime = $event)
          "
          dense
          label="Total Time"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-input
          class="q-pa-md q-pt-none col-12"
          filled
          type="textarea"
          dense
          :model-value="
            editedRecipe.ingredientEntered
              ? editedRecipe.ingredientEntered
              : ingredientEntered
          "
          label="Ingredients - press enter to add each ingredient...*"
          lazy-rules
          @update:model-value="ingredientToAdd($event)"
          @keyup.enter="addToIngredients($event)"
          :rules="[
            () =>
              this.ingredients.length > 0 ||
              this.editedRecipe.ingredients.length > 0 ||
              'Please press enter after each ingredient input',
          ]"
        >
          <div v-if="!editedRecipe.ingredients?.length">
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
          </div>
          <div v-else>
            <q-chip
              v-for="(ingredient, index) in editedRecipe.ingredients"
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
          </div>
        </q-input>

        <q-input
          class="q-pa-md col-12"
          filled
          :model-value="editedRecipe.image ? editedRecipe.image : image"
          @update:model-value="
            editedRecipe.image
              ? (editedRecipe.image = $event)
              : (image = $event)
          "
          dense
          label="Image URL"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-input
          class="q-pa-md col-12"
          filled
          dense
          :model-value="
            editedRecipe.description ? editedRecipe.description : description
          "
          @update:model-value="
            editedRecipe.description
              ? (editedRecipe.description = $event)
              : (description = $event)
          "
          label="Description"
          type="textarea"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <div class="q-pa-md col-12 text-subtitle1">Instructions:</div>
        <div class="col-12 row" v-if="!editedRecipe.instructions">
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
                () => instructions.length > 0 || 'Please type something',
              ]"
            />
          </div>
        </div>
        <div class="col-12 row" v-else>
          <div
            class="col-12 row"
            v-for="(instruction, index) in editedRecipe.instructions"
            :key="index"
          >
            <q-input
              class="q-pa-md col-12"
              filled
              dense
              v-model="editedRecipe.instructions[index]"
              :label="`Step ${index + 1}:`"
              type="textarea"
              lazy-rules
              @update:model-value="instructionToAdd($event, index)"
              @keyup.enter="addToInstructions($event)"
              :rules="[
                () => instructions.length > 0 || 'Please type something',
              ]"
            />
          </div>
        </div>
        <div class="q-pa-md col-12">
          <q-btn
            dense
            color="primary"
            label="Add more steps"
            @click="addStep()"
          />
        </div>

        <q-toggle
          class="col-1"
          :model-value="
            editedRecipe.favourite && editedRecipe.favourite === 1
              ? true
              : editedRecipe.favourite && editedRecipe.favourite === 0
              ? false
              : favourite
          "
          @update:model-value="
            editedRecipe.favourite
              ? (editedRecipe.favourite = $event)
              : (favourite = $event)
          "
          label="Add to favourites"
        />
      </q-form>
      <q-card-actions class="q-pt-xl" align="right">
        <q-btn color="primary" label="Save" @click="onOKClick" type="submit" />
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
      editedRecipe: {},
    };
  },
  mounted() {
    if (this.recipe.id) {
      this.editedRecipe = this.recipe;
    } else {
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
          if (this.editedRecipe.name) {
            this.$emit("ok", {
              editedRecipe: true,
              name: this.editedRecipe.name,
              description: this.editedRecipe.description,
              instructions: this.editedRecipe.instructions,
              favourite: this.editedRecipe.favourite === false ? 0 : 1,
              level: this.editedRecipe.level,
              image: this.editedRecipe.image,
              ingredients: this.editedRecipe.ingredients,
              servingSize: this.editedRecipe.servingSize,
              prepTime: this.editedRecipe.prepTime,
              cookTime: this.editedRecipe.cookTime,
              totalTime: this.editedRecipe.totalTime,
            });
          } else {
            this.$emit("ok", {
              newRecipe: true,
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
          }
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
      if (!this.editedRecipe.name) {
        this.instructions.push("");
      } else {
        this.editedRecipe.instructions.push("");
      }
    },

    removeIngredient(ingredient) {
      if (this.ingredients.length) {
        this.ingredients = this.ingredients.filter(
          (item) => item !== ingredient
        );
      } else {
        this.editedRecipe.ingredients = this.editedRecipe.ingredients.filter(
          (item) => item !== ingredient
        );
      }
    },

    addToIngredients(ingredient) {
      this.editedRecipe.ingredients
        ? this.editedRecipe.ingredients.push(this.ingredient)
        : this.ingredients.push(this.ingredient);
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
    },
  },
};
</script>
