<template>
  <div>
    <input type="text" v-model="searchValue" v-on:keyup.enter="searchText()" />
    <table style="width: 50vw">
      <tr>
        <th>Text</th>
      </tr>
      <tr v-for="item in dataTable" :key="item">
        <td>{{ item }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  head() {
    return {
      title: "Section 2-2",
    };
  },
  data() {
    return {
      searchValue: "",
      search: "",
      dataTable: [],
      items: [],
    };
  },
  watch: {
    searchItems() {
      this.dataTable = [];
      this.items.forEach((item) => {
        console.log("item", item);
        let find = item.search(`${this.search}`);
        if (find >= 0) {
          this.dataTable.push(item);
        }
      });
    },
  },
  computed: {
    searchItems() {
      return [this.search];
    },
  },
  async created() {
    await this.getText();
  },
  methods: {
    searchText() {
      this.search = this.searchValue;
    },
    async getText() {
      const response = await fetch(
        "https://api.publicapis.org/categories"
      ).then((res) => res.json());
      this.items = response;
      this.dataTable = this.items;
    },
  },
};
</script>

<style scoped>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}
</style>
