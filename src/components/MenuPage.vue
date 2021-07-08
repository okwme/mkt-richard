<template>
    <div>
        <template class="nav-section" v-if="lang == 'en'">
            <div v-html="html(doc.body_en)"/>
        </template>
        <template v-else>
            <div v-html="html(doc.body_de)"/>
        </template>
    </div>
</template>

<script>
const md = require('markdown-it')({ html: true })
export default {
  props: ['lang', 'slug'],
  data () {
    return {
      doc: require('../../static/content/menu-pages/' + this.slug + '.json')
    }
  },
  methods: {
    html (markdown) {
      return md.render(markdown)
    }
  }
}
</script>

<style lang="css" scoped>
</style>
