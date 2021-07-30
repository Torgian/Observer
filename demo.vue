<template lang="pug">
div
  v-simple-table
    thead
      tr
        th Title
        th Body
    
    tbody
      tr(v-for="data in renderedData" :key="data.title")
        td {{ data.title }}
        td {{ data.body }}
    
    tfoot
      tr
        td(colspan="2")
          observer(@intersect="queryApi()")

</template>
<script>
import Observer from '~/components/Elements/Observer.vue'

export default {
  name: 'DemoObserve',

  components: {
    Observer,
  },

  data() {
    return {
      currentPage: 0,
      randomData: [],
      renderedData: [],
    }
  },

  mounted() {
    for (let i = 1; i < 101; i++) {
      this.randomData.push({
        title: `Title: ${i}`,
        body: `Body: This is a body. Random cats go in here. Meow, bitches. Woof ${i}`, 
      })
    }

    this.renderedData.push(...this.randomData.slice(0, 10))
  },

  computed: {
    lastPage() {
      return this.randomData.length / 10
    },
  },

  methods: {
    queryApi() {
      if (this.currentPage === this.lastPage) {
        return
      }
      
      this.currentPage += 1
      this.renderedData.push(...this.randomData.slice(this.currentPage * 10, (this.currentPage + 1) * 10))
    },
  },
}
</script>
