<template>
  <v-app>
    <v-content>
      <v-container fluid>
        <v-autocomplete
          ref="autocomplete"
          v-model="selected"
          label="Select"
          :loading="loading"
          :items="items"
          :search-input.sync="search"
        >
          <template v-slot:append-item>
            <div
              v-if="!isLastPage"
              v-observe-visibility="{
                callback: visibilityChanged
              }"
              class="text-xs-center"
            >
              Loading more items ...
            </div>
          </template>
        </v-autocomplete>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
export default {
  name: "app",

  data() {
    return {
      isLastPage: false,
      loading: false,
      items: Array.from(Array(10).keys()),
      selected: "",
      search: ""
    };
  },

  watch: {
    search(val) {
      val && val !== this.selected && this.getItems({ query: val, page: 0 });
    }
  },

  methods: {
    visibilityChanged(e) {
      e && this.getItems();
    },
    getItems() {
      this.loading = true;

      // simulates ajax request
      setTimeout(() => {
        this.items = [
          ...this.items,
          ...Array.from(Array(10).keys()).map(e => this.items.length + e)
        ];
      }, 500);

      // workaround to refresh list after adding new items.
      setTimeout(() => {
        console.log("on scroll");
        this.$refs.autocomplete.onScroll();
      }, 500);

      this.loading = false;
    }
  }
};
</script>
