<template>
  <v-form @submit.prevent="handleSubmit" class="pa-4" ref="formRef">
    <DynamicGroup
      v-for="(group, index) in form.groups"
      :key="index"
      :group="group"
      :canRemove="form.groups.length > 2"
      :removeGroup="() => removeGroup(index)"
    />
    <v-btn color="primary" @click="addGroup" class="mb-4">+ Add More</v-btn>

    <v-text-field
      v-model="form.birthDate"
      label="Birth Date"
      placeholder="June 12, 2024"
      class="mb-4"
    />

    <v-radio-group v-model="form.gender" label="Gender" class="mb-4">
      <v-radio label="Male" value="Male" />
      <v-radio label="Female" value="Female" />
    </v-radio-group>

    <v-checkbox-group v-model="form.languages" label="Languages" class="mb-4">
      <v-checkbox label="VueJs" value="VueJs" />
      <v-checkbox label="ReactJs" value="ReactJs" />
      <v-checkbox label="Angular" value="Angular" />
    </v-checkbox-group>

    <v-select
      v-model="form.cities"
      :items="cities"
      label="Cities"
      multiple
      chips
      class="mb-4"
    />

    <v-file-input
      v-model="form.file"
      label="Upload File"
      accept=".pdf,image/jpeg"
      :rules="[validateFile]"
      class="mb-4"
    />

    <v-btn type="submit" color="success" class="mb-4">Submit</v-btn>

    <SubmittedTable :data="submittedData" />
  </v-form>
</template>

<script setup>
import { ref } from "vue";
import DynamicGroup from "./DynamicGroup.vue";
import SubmittedTable from "./SubmittedTable.vue";

const formRef = ref();

const form = ref({
  groups: [
    { name: "", email: "", mobile: "" },
    { name: "", email: "", mobile: "" },
  ],
  birthDate: "",
  gender: "",
  languages: [],
  cities: [],
  file: null,
});

const cities = [
  "Mumbai",
  "Pune",
  "Delhi",
  "Bangalore",
  "Chennai",
  "Hyderabad",
  "Jaipur",
  "Kolkata",
  "Nagpur",
  "Ahmedabad",
];
const submittedData = ref([]);

const validateFile = (file) => {
  if (!file) return true;
  const allowed = ["application/pdf", "image/jpeg"];
  return allowed.includes(file.type) || "Only JPEG or PDF allowed";
};

function addGroup() {
  form.value.groups.push({ name: "", email: "", mobile: "" });
}

function removeGroup(index) {
  if (form.value.groups.length > 2) form.value.groups.splice(index, 1);
}

async function handleSubmit() {
  const payload = {
    ...form.value,
    fileName: form.value.file?.name || "",
  };

  try {
    const token = "jwt-token";
    const response = await fetch("api-call", {
      method: "POST",
      headers: {
        Authorization: `Bearer ${token}`,
        "Content-Type": "application/json",
      },
      body: JSON.stringify(payload),
    });

    if (!response.ok) throw new Error("API Error: " + response.status);

    submittedData.value.push(payload);
  } catch (err) {
    alert("Submission Failed: " + err.message);
  }
}
</script>
