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
