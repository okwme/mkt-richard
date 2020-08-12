<template>
    <div>
        <template class="nav-section" v-if="lang == 'en'">
            <div v-html="doc.body_en"/>
        </template>
        <template v-else>
            <div v-html="doc.body_de"/>
        </template>
    </div>
</template>

<script>
export default {
  props: ['lang', 'slug'],
  data () {
    return {
      doc: require('../../static/content/menu-pages/' + this.slug + '.json')
    }
  }
}
</script>

<style lang="css" scoped>
</style>
