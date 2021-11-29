<script>
export default {
  name: "filter-dropdown",
  props: {
    columnProp: {
      type: String,
      default: "",
    },
    shown: {
      type: Boolean,
      default: false,
    },
    textInFilterArr: {
      type: Array,
      default: () => {},
    },
    filterText: {
      type: String,
      default: "",
    },
    checkStatus: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    filterTextRender() {
      const { openFilterTooltip } = this.$listeners;
      if (this.columnProp === "id" && this.textInFilterArr[0]) {
        return (
          <div on={{ click: openFilterTooltip }}>
            {" "}
            {`(${this.textInFilterArr[0]})`}
          </div>
        );
      } else if (this.columnProp === "completed" && this.textInFilterArr[1]) {
        if (this.textInFilterArr[1] === true) {
          return <div on={{ click: openFilterTooltip }}> {`Да`} </div>;
        }
        if (this.textInFilterArr[1] === false) {
          return <div on={{ click: openFilterTooltip }}> {`Нет`} </div>;
        } else
          return <div on={{ click: openFilterTooltip }}> {`Не задано`} </div>;
      } else if (this.columnProp === "title" && this.textInFilterArr[2]) {
        return (
          <div on={{ click: openFilterTooltip }}>
            {" "}
            {`(${this.textInFilterArr[2]})`}{" "}
          </div>
        );
      } else if (this.columnProp === "userId" && this.textInFilterArr[3]) {
        return (
          <div on={{ click: openFilterTooltip }}>
            {" "}
            {`(${this.textInFilterArr[3]})`}{" "}
          </div>
        );
      } else return <div> </div>;
    },
    inputRender() {
      const { $style, filterText, $listeners, checkStatus } = this;
      const { setFilterText, setCheckStatus } = $listeners;
      if (this.columnProp === "id" || this.columnProp === "userId") {
        return (
          <input
            type="number"
            class={$style.number}
            min="1"
            domProps={{ value: filterText }}
            on={{ input: setFilterText }}
          />
        );
      } else if (this.columnProp === "completed") {
        return (
          <label class={$style.checkboxLabel}>
            <input
              type="checkbox"
              domProps={{ checked: checkStatus }}
              on={{ input: setCheckStatus }}
            />
            Сделано
          </label>
        );
      } else
        return (
          <input
            type="text"
            class={$style.text}
            domProps={{ value: filterText }}
            on={{ input: setFilterText }}
          />
        );
    },
  },
  render() {
    const { $style, shown, $listeners, filterTextRender } = this;
    const { openFilterTooltip, closeFilterTooltip, searchFilterTooltip } =
      $listeners;

    return (
      <v-dropdown
        class={$style.filterIcon}
        triggers={[]}
        autoHide={false}
        shown={shown}
      >
        <div class={$style.filterWrapper}>
          <font-awesome-icon icon="filter" on={{ click: openFilterTooltip }} />
          {filterTextRender}
        </div>

        <div slot="popper">
          <font-awesome-icon
            icon="times"
            class={$style.closeIcon}
            on={{ click: closeFilterTooltip }}
          />
          {this.inputRender}
          <font-awesome-icon
            icon="search"
            class={$style.searchIcon}
            on={{ click: searchFilterTooltip }}
          />
        </div>
      </v-dropdown>
    );
  },
};
</script>

<style module>
.filterIcon {
  margin-left: auto;
  margin-right: 8px;
}

.filterIcon:hover {
  cursor: pointer;
}

.closeIcon {
  margin-right: 6px;
  font-size: 20px;
}

.closeIcon:hover {
  cursor: pointer;
  color: #f91155;
}
.searchIcon {
  margin-left: 6px;
  font-size: 20px;
}

.searchIcon:hover {
  cursor: pointer;
  color: #005bff;
}
input {
  font-size: 1.2rem;
}
.number {
  width: 60px;
}
.checkboxLabel {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
.filterWrapper {
  display: flex;
  flex-direction: row;
  align-items: center;
  width: 40px;
}
</style>
