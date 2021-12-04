<template>
  <div id="app">
    <div class="btn-wrapper">
      <h1>Выбери пресет для отображения таблицы ToDo</h1>
      <button class="btn sort" v-if="!sortSample" v-on:click="btnHandler">
        С сортировкой
      </button>
      <button class="btn paging" v-if="!pagingSample" v-on:click="btnHandler">
        С пагинацией
      </button>
      <button class="btn scroll" v-if="!scrollSample" v-on:click="btnHandler">
        С бесконечной прокруткой
      </button>
    </div>

    <div class="table-wrapper">
      <div v-if="sortSample">
        <h1>Таблица с сортировкой</h1>
        <h3>
          (Фильтруется каждая колонка по отдельности, а сортироваться могут
          попарно 1 и 3, 2 и 4)
        </h3>
        <WithSort />
      </div>

      <div v-if="pagingSample">
        <h1>Таблица с пагинацией</h1>
        <WithPaging />
      </div>
      <div v-if="scrollSample">
        <h1>Таблица с "бесконечной" прокруткой</h1>
        <WithScroll />
      </div>
    </div>
  </div>
</template>

<script>
import WithSort from "@/components/with-sort";
import WithPaging from "@/components/with-paging";
import WithScroll from "@/components/with-scroll";

export default {
  name: "App",
  components: {
    WithSort,
    WithPaging,
    WithScroll,
  },
  data() {
    return {
      sortSample: true,
      pagingSample: false,
      scrollSample: false,
    };
  },
  methods: {
    btnHandler(evt) {
      if (evt.target.className === "btn sort") {
        this.sortSample = !this.sortSample;
        this.pagingSample = false;
        this.scrollSample = false;
      }
      if (evt.target.className === "btn paging") {
        this.pagingSample = !this.pagingSample;
        this.sortSample = false;
        this.scrollSample = false;
      }
      if (evt.target.className === "btn scroll") {
        this.scrollSample = !this.scrollSample;
        this.pagingSample = false;
        this.sortSample = false;
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  font-size: 20px;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #000000;
  margin: 50px auto;
  min-width: 1000px;
}
.table-wrapper {
  margin-top: 100px;
}

.btn {
  align-items: center;
  background-color: #005bff;
  border-radius: 6px;
  border: 1px solid;
  box-sizing: border-box;
  font-size: 20px;
  color: #fff;
  display: inline-flex;
  justify-content: center;
  min-height: 40px;
  min-width: 96px;
  padding: 0 12px;
  text-align: center;
}
.btn:hover {
  cursor: pointer;
}
</style>
