<template>
  <q-layout>
    <div slot="header" class="toolbar">
      <q-toolbar-title :padding="0">
        Search Demo
      </q-toolbar-title>
    </div>
    
    <div class="row justify-center items-center width-1of1">
      <div>
        <search-box></search-box>
      </div>
    </div>

  </q-layout>
</template>

<script>
import searchBox from 'components/searchBox.vue'

export default {
  data () {
    return {
    }
  },
  computed: {
  },
  methods: {
  },
  mounted () {
  },
  beforeDestroy () {
  },
  components: {
    searchBox
  }
}
</script>

<style lang="styl">
.logo-container
  width 192px
  height 268px
  perspective 800px
  position absolute
  top 50%
  left 50%
  transform translateX(-50%) translateY(-50%)
.logo
  position absolute
  transform-style preserve-3d
</style>
