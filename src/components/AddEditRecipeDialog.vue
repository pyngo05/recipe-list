<template>
  <q-dialog ref="dialog" @hide="onDialogHide" full-width>
    <q-card class="q-dialog-plugin">
      <div class="text-h4 q-pa-lg text-center text-primary text-bold">
        {{ editedRecipe.name ? "Edit Recipe" : "Add Recipe" }}
      </div>

      <q-form @submit="onOKClick">
        <q-input
          class="q-mb-md col-12 col-sm-6"
          filled
          v-model="editedRecipe.name"
          label="Recipe Name"
          dense
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-select
          class="q-mb-md col-12 col-sm-6"
          filled
          dense
          v-model="editedRecipe.level"
          :options="options"
          label="Level"
          :rules="[(val) => val !== null || 'Please select level']"
        />

        <q-input
          class="q-mb-md col-12 col-sm-6"
          filled
          v-model="editedRecipe.servingSize"
          label="Serving Size"
          dense
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-input
          class="q-mb-md col-12 col-sm-6"
          filled
          v-model="editedRecipe.prepTime"
          label="Prep Time"
          dense
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-input
          class="q-mb-md col-12 col-sm-6"
          filled
          v-model="editedRecipe.cookTime"
          label="Cook Time"
          dense
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-input
          class="q-mb-md col-12 col-sm-6"
          filled
          v-model="editedRecipe.totalTime"
          label="Total Time"
          dense
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <div class="q-mb-md col-12">
          <q-input
            filled
            v-model="ingredientEntered"
            label="Ingredients - press enter to add each ingredient...*"
            :rules="[
              () =>
                ingredients.length > 0 ||
                editedRecipe.ingredients.length > 0 ||
                'Please press enter after each ingredient input',
            ]"
            @update:model-value="ingredientToAdd"
            @keyup.enter="addToIngredients"
          />
          <div class="q-chips-container">
            <q-chip
              v-for="(ingredient, index) in ingredients.length
                ? ingredients
                : editedRecipe.ingredients"
              :key="index"
              dense
              removable
              color="primary"
              text-color="white"
              @remove="removeIngredient(ingredient)"
            >
              {{ ingredient }}
            </q-chip>
          </div>
        </div>

        <q-input
          class="q-mb-md col-12"
          filled
          v-model="editedRecipe.image"
          label="Image URL"
          dense
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-input
          class="q-mb-md col-12"
          filled
          v-model="editedRecipe.description"
          label="Description"
          type="textarea"
          dense
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
              class="q-mb-md col-12"
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
              class="q-mb-md col-12"
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
          class="q-mb-md col-12"
          v-model="editedRecipe.favourite"
          :label="
            editedRecipe.favourite
              ? 'Remove from favorites'
              : 'Add to favorites'
          "
        />
      </q-form>

      <q-card-actions class="q-pt-xl" align="right">
        <q-btn color="primary" label="Save" @click="onOKClick" />
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
      options: ["Easy", "Medium", "Hard"],
      ingredients: [],
      instructions: ["", "", "", "", ""],
      ingredientEntered: "",
      editedRecipe: {
        name: "",
        description: "",
        instructions: [],
        favourite: false,
        level: null,
        image: "",
        servingSize: "",
        prepTime: "",
        cookTime: "",
        totalTime: "",
      },
    };
  },
  mounted() {
    if (this.recipe.id) {
      this.editedRecipe = this.recipe;
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
              ...this.editedRecipe,
              favourite: this.editedRecipe.favourite ? 1 : 0,
            });
          } else {
            this.$emit("ok", {
              newRecipe: true,
              ...this.editedRecipe,
              favourite: this.editedRecipe.favourite ? 1 : 0,
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
      this.editedRecipe.instructions.push("");
    },

    removeIngredient(ingredient) {
      this.editedRecipe.ingredients = this.editedRecipe.ingredients.filter(
        (item) => item !== ingredient
      );
    },

    addToIngredients() {
      if (this.ingredientEntered.trim()) {
        this.editedRecipe.ingredients.push(this.ingredientEntered.trim());
        this.ingredientEntered = "";
      }
    },

    ingredientToAdd(ingredient) {
      this.ingredientEntered = ingredient;
    },

    instructionToAdd(instruction, index) {
      this.editedRecipe.instructions[index] = instruction;
    },

    addToInstructions(instruction) {
      this.instructions.push(instruction);
    },
  },
};
</script>
