<template>
  <v-table v-if="data.length" class="mt-8">
    <thead>
      <tr>
        <th>#</th>
        <th>Names</th>
        <th>Emails</th>
        <th>Mobiles</th>
        <th>Birth Date</th>
        <th>Gender</th>
        <th>Languages</th>
        <th>Cities</th>
        <th>File</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(entry, i) in data" :key="i">
        <td>{{ i + 1 }}</td>
        <td>{{ entry.groups.map((g) => g.name).join(", ") }}</td>
        <td>{{ entry.groups.map((g) => g.email).join(", ") }}</td>
        <td>{{ entry.groups.map((g) => g.mobile).join(", ") }}</td>
        <td>{{ entry.birthDate }}</td>
        <td>{{ entry.gender }}</td>
        <td>{{ entry.languages.join(", ") }}</td>
        <td>{{ entry.cities.join(", ") }}</td>
        <td>{{ entry.fileName }}</td>
      </tr>
    </tbody>
  </v-table>
</template>

<script setup>
defineProps(["data"]);
</script>
