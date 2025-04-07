<script>
import inputTag from "./components/inputTag.vue";
export default {
  components: {
    inputTag,
  },
  methods: {
    handleTagsChange(tags) {
      this.tags = tags;
      console.log(this.tags);
    },
  },
};
</script>

<template>
  <inputTag @onTagsChange="handleTagsChange" />
</template>

<style scoped>

</style>