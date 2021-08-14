# multiselect-lib

## installation

```
yarn add m-multiselect-lib
or
npm install --save-dev m-multiselect-lib
```

## Example Usage

```
<template>
<div>
<multiselect
    inputStyle="input-class-style"
    selectStyle="select-style-class"
    tagStyle="tag-style-class"
    optionStyle="option-style-class"
    :options="optionList"
    optionName="name"
    optionValue="value"
    :value="values"
    @change="(v) => changeData(v)"
/>
</div>
</template>

<script lang="ts">
import Multiselect from "m-multiselect-lib";

export default Vue.extend({
  components: { Multiselect },
  data() {
    return {
      optionList: [
        { name: "x", value: "x" },
        { name: "y", value: "y" },
      ],
      values,
    };
  },
  methods: {
    changeData(d: any) {
      this.values = [...d];
    },
  },
});
</script>
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).
