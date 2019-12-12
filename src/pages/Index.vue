<template>
  <q-page class="minorMargin minorPadding">
    <q-select
      dense
      v-model="filterType"
      use-input
      input-debounce="0"
      :options="filterOptions"
      @filter="filterFn"
      label="Filter on type"
      style="width: 280px"
      behavior="menu"
      outlined
      clearable
    >
      <template v-slot:no-option>
        <q-item>
          <q-item-section class="text-grey">No results</q-item-section>
        </q-item>
      </template>
    </q-select>

    <div v-if="items" class="minorPadding">
      <div
        v-for="item in items.filter(x=> {return filterType == null || x.layoutType == filterType})"
        :key="item.id"
      >
        <buildingInfoBox :data="item" />
      </div>
    </div>
  </q-page>
</template>

<script>
import buildingInfoBox from "../components/buildingInfoBox";
import axios from "axios";
export default {
  name: "PageIndex",
  components: {
    buildingInfoBox
  },
  data() {
    return {
      items: null,
      filterType: null,
      filterOptions: this.getTypes
    };
  },
  computed: {
    getTypes() {
      var ls = [];
      if (this.items) {
        this.items.forEach(item => {
          if (ls.filter(x => x == item.layoutType).length == 0) {
            ls.push(item.layoutType);
          }
        });
      }

      return ls;
    }
  },
  methods: {
    filterFn(val, update, abort) {
      if (val === "") {
        update(() => {
          this.filterOptions = this.getTypes;
        });
        return;
      }

      update(() => {
        const needle = val.toLowerCase();
        this.filterOptions = this.getTypes.filter(
          v => v.toLowerCase().indexOf(needle) > -1
        );
      });
    }
  },
  created() {
    var vm = this;
    axios
      .get("https://bygrapi.sindrema.com/api/buyer-info")
      .then(x => {
        if (x.data) {
          vm.items = x.data;
        }
      })
      .catch(x => {
        console.log(x);
      });
  }
};
</script>

<style scoped>
.minorMargin {
  margin: 20px;
}

.minorBorder {
  border: rgb(160, 160, 160) 1px solid;
  border-radius: 10px;
}
.minorPadding {
  padding: 20px;
}
</style>
