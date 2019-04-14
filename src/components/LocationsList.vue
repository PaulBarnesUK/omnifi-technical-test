<template>
  <ul class="locations">
    <li class="location" v-for="(location, index) in locations" @click="handleLocationSelect(location)" :key="index">
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
  .location {
    border-bottom: 1px solid #cccccc;
    cursor: pointer;
    list-style-type: none;
    padding: 15px 20px;
    transition: all .3s;

    &:hover {
      background-color: lighten(#cccccc, 15%);
    }
  }

  .locations {
    flex: 0 0 25%;
    max-width: 25%;

    box-sizing: border-box;
    margin: 0;
    overflow: auto;
    padding: 0;
  }
</style>
