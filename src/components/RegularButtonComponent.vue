<template>
  <button type="button">{{ buttonName }}</button>
</template>

<script>
export default {
  name: "RegularButtonComponent",
  props: {
    buttonName: String,
  },
};
</script>

<style scoped>
button {
  margin-left: 2rem;
}
</style>
