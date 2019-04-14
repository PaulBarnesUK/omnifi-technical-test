<template>
  <ul class="locations">
    <li v-for="(location, index) in locations" @click="handleLocationSelect(location)" :key="index">
      {{ location.name }}
    </li>
  </ul>
</template>

<script>
export default {
  props: [
    'locations'
  ],

  methods: {
    handleLocationSelect (location) {
      this.$emit('locationSelected', location)
    }
  }
}
</script>

<style lang="scss">
  .locations {
    flex: 0 0 25%;
    max-width: 25%;

    box-sizing: border-box;
    margin: 0;
    overflow: auto;
    padding: 20px 30px;
  }
</style>
