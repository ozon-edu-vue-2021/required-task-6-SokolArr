<template>
  <v-table
    :rows="rows"
    :total-pages="10"
    :current-page="currentPage"
    :static-paging="false"
    @getPage="infGetPage"
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
    };
  },
  methods: {
    async getPage(number) {
      const res = await fetch(
        `https://jsonplaceholder.typicode.com/todos?userId=${number}`
      );
      this.rows = await res.json();
      this.currentPage = number;
    },
    async infGetPage() {
      this.blockingPromise && (await this.blockingPromise);
      const res = await fetch(
        `https://jsonplaceholder.typicode.com/todos?userId=${
          this.currentPage + 1
        }`
      );
      const newRows = await res.json();
      this.rows = [...this.rows, ...newRows];
      this.currentPage++;
    },
  },
};
</script>
