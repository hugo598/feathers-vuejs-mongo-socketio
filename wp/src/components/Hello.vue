<template>
  <div class="hello">
    <ul>
      <li v-for="msg in msgs">
        {{msg}}
      </li>
    </ul>

  </div>
</template>

<script>
export default {
  name: 'hello',
  data () {
    return {
      msgs: [1,2,3]
    }
  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
