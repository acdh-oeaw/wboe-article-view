<template>
  <span :id="'gs' + content.uId">
    <template v-for="(place, pKey) in placesView()">
      <span
        :key="'pKey+' + pKey"
        :data-geo-sigle="getPlacenameSigleFromRef(place.orgXmlObj.attributes.ref)"
        v-text="
          (pKey > 0 ? content.fxData.join : '')
          + place.orgXmlObj.getValue(false)
          + maybeGrossregion(place)" />
    </template>
  </span>
</template>

<script>
  export default {
    name: 'GeoPreview',
    props: {
      content: Object,
      geoStore: Object
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
      getGrossregionFromSigle (sigle) {
        if (this.geoStore && this.geoStore.grossregionen) {
          const s = sigle.split(/([a-z])/)[0]
          const g = this.geoStore.grossregionen.features.find((f) => {
            return f.properties.Sigle === s
          })
          return g ? g.properties.Grossreg : null
        } else {
          return null
        }
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
          place.orgXmlObj !== undefined
          && place.orgXmlObj.attributes !== undefined
          && place.orgXmlObj.attributes.ref !== undefined
        ) {
          // TODO: resolve the Großregion
          let out = null
          const sigle = this.getPlacenameSigleFromRef(place.orgXmlObj.attributes.ref) || null
          if (place.orgXmlObj.attributes.type === 'gemeinde') {
            out = this.getGrossregionFromSigle(sigle)
          } else if (place.orgXmlObj.attributes.type === 'kleinregion') {
            out = this.getGrossregionFromSigle(sigle)
          }
          return out
            ? ', ' + out
            : ''
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
