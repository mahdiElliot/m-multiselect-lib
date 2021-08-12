<template>
    <component :is="'my-' + name" @click="(e) => $emit('click', e)" />
</template>

<script>
export default{
  components: {
    "my-arrow-down": () => import("../assets/ic_arrow_down.svg"),
  },
  props: {
    name: {
      type: String,
      default: () => "grid",
    },
  },
};
</script>

<style></style>
