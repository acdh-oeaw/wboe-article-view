<template>
  <span :id="'gs' + content.uId"><template v-for="(place, pKey) in placesView()"><template v-if="pKey > 0">{{ content.fxData.join }}</template>{{ place.orgXmlObj.getValue(false) + maybeGrossregion(place) }}</template></span>
</template>

<script>
  export default {
    name: 'GeoPreview',
    props: {
      content: Object,
    },
    data () {
      return {
        'placesEdit': [],
      }
    },
    computed: {
    },
    watch: {
    },
    mounted () {
      console.log(this.content, this.placesView())
    },
    methods: {
      placesView () {
        return this.content.getChilds('all', true).filter(c => {
          return c.orgXmlObj.name === 'placeName' && c.orgXmlObj.getValue(false)
        })
      },
      last (array) {
        return array[array.length - 1]
      },
      getPlacenameSigleFromRef (ref) {
        if (ref === null) {
          return null
        } else {
          return this.last(ref.split(/([#p])/)) || null
        }
      },
      maybeGrossregion (place) {
        if (
          place.orgXmlObj !== undefined &&
          place.orgXmlObj.attributes !== undefined &&
          place.orgXmlObj.attributes.ref !== undefined && 
          place.orgXmlObj.attributes.type === 'gemeinde'
        ) {
          // TODO: resolve the Großregion
          // return this.getPlacenameSigleFromRef(place.orgXmlObj.attributes.ref) || ''
          return ''
        } else {
          return ''
        }
      }
    },
    components: {
    },
  }
</script>

<style scoped>
</style>
