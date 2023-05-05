<template>
  <q-dialog ref="dialog" @hide="onDialogHide" full-width full-height>
    <q-card class="q-dialog-plugin">
      <q-form @submit="onOKClick" class="q-gutter-md" ref="form">
        <q-input
          class="q-pa-md"
          filled
          v-model="name"
          label="Recipe Name"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />
        <q-input
          class="q-pa-md"
          filled
          v-model="image"
          label="Image URL"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />
        <q-select
          class="q-pa-md"
          filled
          v-model="level"
          :options="options"
          label="Level"
          :rules="[(val) => val !== null || 'Please select level']"
        />

        <q-input
          class="q-pa-md"
          filled
          v-model="description"
          label="Description"
          type="textarea"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-input
          class="q-pa-md"
          filled
          v-model="instructions"
          label="Instructions"
          type="textarea"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-toggle v-model="favourite" label="Add to favourites" />
      </q-form>
      <q-card-actions align="right">
        <q-btn color="primary" label="OK" @click="onOKClick" type="submit" />
        <q-btn color="primary" label="Cancel" @click="onCancelClick" />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script>
export default {
  name: "AddRecipeDialog",
  props: {},
  emits: ["ok", "hide"],
  data() {
    return {
      name: null,
      description: null,
      instructions: null,
      favourite: false,
      level: null,
      image: null,
      options: ["Easy", "Medium", "Hard"],
    };
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
  },
};
</script>
