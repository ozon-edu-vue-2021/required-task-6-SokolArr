<template>
  <v-table :rows="rows">
    <v-table-column prop="id" title="№" />

    <v-table-column prop="completed">
      <template #title>
        <b>Сделано</b>
      </template>

      <template #body="{ row }">
        <div v-if="row.completed">✅ - да</div>
        <div v-else>❌ - нет</div>
      </template>
    </v-table-column>

    <v-table-column prop="title" title="Описание задания" />
    <v-table-column prop="userId" title="ID пользователя" />
  </v-table>
</template>

<script>
import VTable from "./v-table";
import VTableColumn from "./v-table-column";

export default {
  name: "SortWrapper",
  components: {
    VTableColumn,
    VTable,
  },
  async created() {
    const res = await fetch(`https://jsonplaceholder.typicode.com/todos`);
    this.rows = await res.json();
  },
  data() {
    return {
      rows: [],
    };
  },
};
</script>
