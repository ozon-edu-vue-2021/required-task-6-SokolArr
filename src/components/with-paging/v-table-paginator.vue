<script lang="jsx">
export default {
  name: "v-table-paginator",
  props: {
    totalPages: {
      type: Number,
      default: 0,
    },
    currentPage: {
      type: Number,
      default: 0,
    },
  },
  computed: {
    shownPagesNumbers() {
      const { currentPage, totalPages } = this;

      if (currentPage <= 3) {
        if (totalPages < 5) {
          return Array(totalPages)
            .fill(null)
            .map((index) => index + 1);
        }

        return [1, 2, 3, 4, 5];
      }

      if (currentPage >= totalPages - 2) {
        return [
          totalPages - 4,
          totalPages - 3,
          totalPages - 2,
          totalPages - 1,
          totalPages,
        ];
      }

      return [
        currentPage - 2,
        currentPage - 1,
        currentPage,
        currentPage + 1,
        currentPage + 2,
      ];
    },
  },
  render() {
    const { $style, shownPagesNumbers, currentPage, totalPages, $listeners } =
      this;
    const { getPage } = $listeners;

    return (
      <div class={$style.pagination}>
        <span class={$style.text} on={{ click: () => getPage(1) }}>
          {"В начало"}
        </span>
        <span
          class={$style.text}
          on={{ click: () => getPage(currentPage - 1) }}
        >
          {"Назад"}
        </span>

        {...shownPagesNumbers.map((number) => (
          <span
            class={[
              $style.numbers,
              { [$style.active]: currentPage === number },
            ]}
            on={{ click: () => getPage(number) }}
          >
            {number}
          </span>
        ))}

        <span
          class={$style.text}
          on={{ click: () => getPage(currentPage + 1) }}
        >
          {"Следующая"}
        </span>
        <span class={$style.text} on={{ click: () => getPage(totalPages) }}>
          {"В конец"}
        </span>
      </div>
    );
  },
};
</script>

<style module>
.pagination {
  height: 48px;
  width: fit-content;
  color: white;
  display: flex;
  align-items: center;
  margin: auto;
}

.text {
  width: auto;
  height: fit-content;
  margin-right: 8px;
  background: #005bff;
  border: 1px solid #c8cacc;
  border-radius: 6px;
  padding: 6px;
  cursor: pointer;
}
.numbers {
  min-width: 20px;
  width: auto;
  height: fit-content;
  margin-right: 8px;
  background: #005bff;
  border: 1px solid #c8cacc;
  border-radius: 6px;
  padding: 6px;
  cursor: pointer;
}
.numbers.active {
  width: 20px;
  background: #f91155;
  color: white;
  border: 1px solid #c8cacc;
  border-radius: 6px;
}
</style>
