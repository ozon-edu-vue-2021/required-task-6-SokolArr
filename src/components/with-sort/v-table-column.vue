<script>
export default {
  name: "v-table-column",
  props: {
    prop: {
      type: String,
      required: true,
    },
    title: {
      type: String,
      default: "",
    },
  },
};
</script>
