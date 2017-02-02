<template>
  <div>
    <div style="padding-bottom:20px;">See console for resulting URL and Objects</div>
    <div class="floating-label">
      <textarea v-model="localities" required class="full-width" style="height: 40px;"></textarea>
      <label>Localities (comma separated)</label>
    </div>
    <q-autocomplete :delay=500 v-model="terms" @search="getAddresses" :min-characters="3" set-width @input="triggerMap(arguments)">
      <q-search v-model="terms" class="primary search-field" placeholder="Find address..."></q-search>
    </q-autocomplete>
    <!-- <div style="text-align: center; padding-top: 20px;">{{ terms || "No Result"}}</div> -->
  </div>
</template>

<script>
import axios from 'axios'
import { Events, Utils } from 'quasar'

export default {
  data () {
    return {
      terms: '',
      options: '',
      localities: 'Warfield'
    }
  },
  computed: {
  },
  methods: {
    getAddresses (search, done) {
      if (search.length > 0) {
        // console.log('Making request for:', search)
        axios.get('https://geocoder.api.gov.bc.ca/addresses.geojson', {
          params: {
            addressString: search,
            localities: this.localities,
            minScore: 20,
            echo: false,
            locationDescriptor: 'parcelPoint',
            maxResults: 50
          },
          headers: {
            apikey: '<apikey>'
          }
        })
        .then(response => {
          // ensure only new data results in options update
          console.log(response.request.responseURL, response.data.features)
          if (!this.searchTime || response.searchTimestamp > this.searchTime) {
            const options = []
            response.data.features.forEach(feature => {
              feature.label = `${feature.properties.civicNumber} ${feature.properties.streetName} ${feature.properties.streetType}`
              feature.secondLabel = `${feature.properties.localityName}, ${feature.properties.provinceCode}`
              feature.value = feature.properties.fullAddress
              options.push(feature)
            })
            this.options = options
            done(options)
            // console.log('Items found:', this.options)
          }
          else { console.info('Old Search Data Received') }
          // console.log(response.data)
        })
      }
      else {
        this.noOptions = 'New Seach'
      }
    },
    triggerMap (feature) {
      if (feature) {
        Events.$emit('map/search-result', Utils.filter(feature[0], {field: 'value', list: this.options})[0])
      }
    }
  },
  components: {
  }
}
</script>

<style lang="stylus">
.search-field
  width 40vw
  
@media screen and (max-width: 599px)
  .search-field
    width 100%
</style>
