<template>
  <v-table
    :rows="rows"
    :total-pages="totalPages"
    :current-page="currentPage"
    @getPage="getPage"
  >
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
  name: "PagingWrapper",
  components: {
    VTableColumn,
    VTable,
  },
  created() {
    this.blockingPromise = this.getPage(1);
  },
  data() {
    return {
      rows: [],
      currentPage: 1,
      totalPages: 10,
    };
  },
  methods: {
    async getPage(number) {
      if (number > 0 && number <= this.totalPages) {
        const res = await fetch(
          `https://jsonplaceholder.typicode.com/todos?userId=${number}`
        );
        this.rows = await res.json();
        this.currentPage = number;
      }
    },
  },
};
</script>
