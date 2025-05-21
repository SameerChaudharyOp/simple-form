<template>
  <v-row class="mb-4" dense>
    <v-col cols="12" sm="4">
      <v-text-field v-model="group.name" label="Name" />
    </v-col>
    <v-col cols="12" sm="4">
      <v-text-field v-model="group.email" label="Email" />
    </v-col>
    <v-col cols="12" sm="4">
      <v-text-field
        v-model="group.mobile"
        label="Mobile Number"
        maxlength="10"
        :rules="[validatePhone]"
        @input="onInput"
      />
    </v-col>
    <v-col cols="12" class="text-right" v-if="canRemove">
      <v-btn icon color="error" @click="removeGroup">
        <v-icon>mdi-delete</v-icon>
      </v-btn>
    </v-col>
  </v-row>
</template>

<script setup>
const props = defineProps(["group", "canRemove", "removeGroup"]);


const validatePhone = (v) =>
  /^\d{10}$/.test(v) || "Mobile number must be 10 digits";
const onInput = () => {
  group.mobile = group.mobile.replace(/\D/g, "");
};
</script>
