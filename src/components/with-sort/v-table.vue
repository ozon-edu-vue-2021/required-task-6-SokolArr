<script>
import { orderBy } from "lodash/collection";
import FilterDropdown from "../dropdown/filter-dropdown.vue";

export default {
  name: "v-table",
  props: {
    rows: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      sortProp: "",
      sortProp1: "",
      sortProp2: "",
      sortProp3: "",
      sortDirection: "",
      sortDirection1: "",
      sortDirection2: "",
      sortDirection3: "",
      filterProp: "",
      filterProp1: "",
      filterProp2: "",
      filterProp3: "",
      filterProp4: "",
      filterText: "",
      filterText1: "",
      filterText2: undefined,
      filterText3: "",
      filterText4: "",
      filterBool: "",
      filterNumber: "",
      checkStatus: false,
    };
  },
  computed: {
    sortedRows() {
      let res;

      if (!this.sortProp) {
        res = this.rows;
      }

      if (this.sortProp || this.sortProp2) {
        res = orderBy(
          orderBy(this.rows, [this.sortProp], [this.sortDirection]),
          [this.sortProp2],
          [this.sortDirection2]
        );
      }
      if (this.sortProp1 || this.sortProp3) {
        res = orderBy(
          orderBy(this.rows, [this.sortProp1], [this.sortDirection1]),
          [this.sortProp3],
          [this.sortDirection3]
        );
      }

      if (this.filterText1) {
        res = res.filter(
          (row) => row[this.filterProp1] === parseInt(this.filterText1)
        );
      }
      if (this.filterText2 != undefined) {
        if (this.checkStatus == undefined) return (res = this.rows);
        res = res.filter((row) => {
          return Number(row[this.filterProp2]) === Number(this.filterText2);
        });
      }

      if (this.filterText3) {
        res = res.filter(
          (row) =>
            row[this.filterProp3].toString().search(this.filterText3) > -1
        );
      }
      if (this.filterText4) {
        res = res.filter((row) => {
          return row[this.filterProp4] === parseInt(this.filterText4);
        });
      }

      return res;
    },
  },
  methods: {
    toggleSort(prop) {
      if (prop === "id") {
        this.sortProp = prop;
        this.sortProp1 = "";
        this.sortProp3 = "";
        this.sortDirection =
          this.sortDirection === "desc" || !this.sortDirection ? "asc" : "desc";
      }
      if (prop === "completed") {
        this.sortProp1 = prop;
        this.sortProp = "";
        this.sortProp2 = "";

        this.sortDirection1 =
          this.sortDirection1 === "desc" || !this.sortDirection1
            ? "asc"
            : "desc";
      }
      if (prop === "title") {
        this.sortProp2 = prop;
        this.sortProp1 = "";
        this.sortProp3 = "";
        this.sortDirection2 =
          this.sortDirection2 === "desc" || !this.sortDirection2
            ? "asc"
            : "desc";
      }
      if (prop === "userId") {
        this.sortProp3 = prop;
        this.sortProp = "";
        this.sortProp2 = "";
        this.sortDirection3 =
          this.sortDirection3 === "desc" || !this.sortDirection3
            ? "asc"
            : "desc";
      }
    },
    openFilterTooltip(prop = "") {
      this.filterProp = prop;
      this.filterText = "";

      if (prop === "id") {
        this.filterText = this.filterText1;
        this.filterProp1 = prop;
      }
      if (prop === "completed") {
        this.checkStatus = this.filterText2;
        this.filterProp2 = prop;
      }
      if (prop === "title") {
        this.filterText = this.filterText3;
        this.filterProp3 = prop;
      }
      if (prop === "userId") {
        this.filterText = this.filterText4;
        this.filterProp4 = prop;
      }
    },
    searchFilterTooltip(prop) {
      if (prop === "id") {
        this.filterText1 = this.filterText;
      }
      if (prop === "completed") {
        this.filterText2 = this.checkStatus;
      }
      if (prop === "title") {
        this.filterText3 = this.filterText;
      }
      if (prop === "userId") {
        this.filterText4 = this.filterText;
      }
      this.openFilterTooltip();
    },
    closeFilterTooltip(prop) {
      if (prop === "id") {
        this.filterText1 = "";
      }
      if (prop === "completed") {
        this.checkStatus = undefined;
        this.filterText2 = this.checkStatus;
      }
      if (prop === "title") {
        this.filterText3 = "";
      }
      if (prop === "userId") {
        this.filterText4 = "";
      }
      this.openFilterTooltip();
    },
    setFilterText(e) {
      this.filterText = e.target.value;
    },
    setCheckStatus(e) {
      this.checkStatus = e.target.checked;
    },

    renderHead(h, columnsOptions) {
      const {
        $style,
        sortProp,
        sortProp1,
        sortProp2,
        sortProp3,
        sortDirection,
        sortDirection1,
        sortDirection2,
        sortDirection3,
        filterProp,
        filterText,
        checkStatus,
      } = this;

      return columnsOptions.map((column) => {
        const renderedTitle = column.scopedSlots.title
          ? column.scopedSlots.title()
          : column.title;
        let sortIcon = "sort";

        if (sortProp === column.prop) {
          sortIcon =
            sortDirection === "asc" ? "sort-amount-down" : "sort-amount-up";
        }
        if (sortProp1 === column.prop) {
          sortIcon =
            sortDirection1 === "asc" ? "sort-amount-down" : "sort-amount-up";
        }
        if (sortProp2 === column.prop) {
          sortIcon =
            sortDirection2 === "asc" ? "sort-amount-down" : "sort-amount-up";
        }
        if (sortProp3 === column.prop) {
          sortIcon =
            sortDirection3 === "asc" ? "sort-amount-down" : "sort-amount-up";
        }
        return (
          <th key={column.prop} class={$style.headerCell}>
            <div class={$style.headerCellContent}>
              <span>{renderedTitle}</span>
              <font-awesome-icon
                class={$style.sortIcon}
                icon={sortIcon}
                on={{ click: () => this.toggleSort(column.prop) }}
              />
              <FilterDropdown
                class={$style.vpopper}
                columnProp={column.prop}
                shown={column.prop === filterProp}
                filterText={filterText}
                checkStatus={checkStatus}
                textInFilterArr={[
                  this.filterText1,
                  this.filterText2,
                  this.filterText3,
                  this.filterText4,
                ]}
                on={{
                  openFilterTooltip: () => this.openFilterTooltip(column.prop),
                  setFilterText: this.setFilterText,
                  setCheckStatus: this.setCheckStatus,
                  closeFilterTooltip: () =>
                    this.closeFilterTooltip(column.prop),
                  searchFilterTooltip: () =>
                    this.searchFilterTooltip(column.prop),
                }}
              />
            </div>
          </th>
        );
      });
    },
    renderRows(h, columnsOptions) {
      return this.sortedRows.map((row, index) => {
        return (
          <tr key={row.id || index}>
            {...this.renderColumns(h, row, columnsOptions)}
          </tr>
        );
      });
    },
    renderColumns(h, row, columnsOptions) {
      return columnsOptions.map((column) => {
        return (
          <td key={column.prop} class={this.$style.cell}>
            {column.scopedSlots.body
              ? column.scopedSlots.body({ row })
              : row[column.prop]}
          </td>
        );
      });
    },
    getColumnOptions() {
      return this.$slots.default
        .filter(
          (item) =>
            item.componentOptions &&
            item.componentOptions.tag === "v-table-column"
        )
        .map((column) =>
          Object.assign({}, column.componentOptions.propsData, {
            scopedSlots: column.data.scopedSlots || {},
          })
        );
    },
  },
  render(h) {
    const columnsOptions = this.getColumnOptions();
    const columnsHead = this.renderHead(h, columnsOptions);
    const rows = this.renderRows(h, columnsOptions);

    return (
      <table class={this.$style.table}>
        <thead>{...columnsHead}</thead>
        <tbody>{...rows}</tbody>
      </table>
    );
  },
};
</script>

<style module>
.table {
  border-spacing: 0;
  border-collapse: collapse;
  width: 100%;
  background: #ececec;
}

.cell {
  text-align: left;
  border-bottom: 1px solid #c8cacc;
  padding: 1rem 1rem;
}

.headerCell {
  composes: cell;
}

.headerCellContent {
  display: flex;
  align-items: center;
  height: 40px;
  font-size: 1rem;
}

.sortIcon {
  margin-left: 8px;
  margin-right: 24px;
}

.sortIcon:hover {
  cursor: pointer;
}
thead {
  background-color: #005bff;
  color: white;
}
.vpopper {
  margin: 0;
}
</style>
