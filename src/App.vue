<template>
  <div id="app">
    <div class="rounded-md">
      <multiselect
        inputStyle="py-5"
        :options="activities"
        optionName="name"
        optionValue="value"
        :value="data.areas"
        @change="(v) => changeData({ areas: v })"
      />
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import Multiselect from "./components/Multiselect.vue";

const emptyData = {
  areas: [],
};

export default Vue.extend({
  components: { Multiselect },
  name: "App",
  data() {
    return {
      activities: [
        { name: "x", value: "x" },
        { name: "y", value: "y" },
      ],
      data: { areas: [] },
    };
  },
  methods: {
    changeData(d: typeof emptyData | any) {
      this.data = { ...this.data, ...d };
    },
  },
});
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  width: 100%;
  height: 100%;
  padding: 3rem;
  background: honeydew;
}
</style>
