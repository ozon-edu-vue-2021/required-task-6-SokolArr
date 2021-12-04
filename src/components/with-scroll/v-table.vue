<script lang="jsx">
import DotsLoaderIcon from "./dost-loader.svg";

export default {
  name: "v-table",
  props: {
    rows: {
      type: Array,
      default: () => [],
    },
    totalPages: {
      type: Number,
      default: 0,
    },
    currentPage: {
      type: Number,
      default: 0,
    },
  },
  methods: {
    renderHead(h, columnsOptions) {
      return columnsOptions.map((column) => {
        const renderedTitle = column.scopedSlots.title
          ? column.scopedSlots.title()
          : column.title;

        return (
          <th key={column.prop} class={this.$style.headerCell}>
            {renderedTitle}
          </th>
        );
      });
    },
    renderRows(h, columnsOptions) {
      return this.rows.map((row, index) => {
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
    renderInfPager() {
      const directives = [
        {
          name: "detect-viewport",
          value: {
            callback: this.$listeners.getPage,
          },
        },
      ];

      const style = {
        background: `url("${DotsLoaderIcon}") no-repeat center`,
      };

      return <div {...{ class: this.$style.infPager, style, directives }} />;
    },
  },
  render(h) {
    const { $style } = this;
    const columnsOptions = this.getColumnOptions();
    const columnsHead = this.renderHead(h, columnsOptions);
    const rows = this.renderRows(h, columnsOptions);

    return (
      <div>
        <table class={$style.table}>
          <thead>{...columnsHead}</thead>
          <tbody>{...rows}</tbody>
        </table>

        {this.renderInfPager()}
      </div>
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
.infPager {
  width: 100%;
  height: 32px;
}
</style>
