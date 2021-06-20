<template>
  <div style="width: 100%" v-if="!error && !isLoading">
    <l-map
      :zoom="19"
      :center="center"
      :maxZoom="19"
      :options="{ zoomSnap: 0.2 }"
      style="height: calc(100vh - max(17rem, 35vh)); min-height: 30rem; box-sizing: border-box;"
    >
    <l-marker :lat-lng="markerPos" :icon="customMarker"></l-marker>
    <l-tile-layer
      :url="url"
      :attribution="attribution"
    />
    </l-map>
  </div>
  <div 
  v-if="isLoading || error"
  style="height: calc(100vh - max(17rem, 35vh)); 
  min-height: 30rem;
  box-sizing: border-box; 
  background-color: var(--darkGray); 
  opacity: .10;">
  </div>

</template>

<script>
import { LMap, LTileLayer, LMarker } from "@vue-leaflet/vue-leaflet";
import { latLng, icon } from "leaflet";
import { computed, ref, inject } from '@vue/runtime-core';

export default {
  name: "Example",
  components: {
    LMap,
    LTileLayer,
    LMarker
  },

  setup() {
    //Current Problem, Map Component renders first before getting the responce from the geoLoc api
    //Solution, find a way to force this component to wait before setting the longhitude and latitude
    const geoLoc = inject('geoLoc')
    const isLoading = inject('loading')
    const error = inject('error')
    const attribution =  '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors';
    const url = 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png';
    const customMarker = icon({
          iconUrl: require('../assets/img/icon-location.svg'),
          iconSize: [48.6636, 59.2668],
          iconAnchor: [24.3318, 59.2668],
        })
   
    function checkGeoLoc() {
      if(isLoading.value) {
        window.setTimeout(checkGeoLoc, 100); /* to Block the code while waiting for GeoLoc Api response*/
      } else if(error.value ){
        return;
      }
    }
    checkGeoLoc();
    
    // 
    const center = computed(() => {
     
      return latLng(geoLoc.value.location.lat - -.00019 , geoLoc.value.location.lng)

    })
    const markerPos = computed(() => {
      
      return latLng(geoLoc.value.location.lat, geoLoc.value.location.lng)
    })
    return {
      attribution,
      url,
      customMarker,
      center,
      markerPos,
      
      error,
      isLoading
    }
  }
  // data() {
  //   return {
  //     attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
  //     url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
  //     customMarker: icon({
  //       iconUrl: require('../assets/img/icon-location.svg'),
  //       iconSize: [48.6636, 59.2668],
  //       iconAnchor: [24.3318, 59.2668],
  //     }),
  //     latitude: 14.58691,
  //     longhitude: 121.0614, 
      
  //   }
  // },
  // computed: {
  //   center() {
  //     return latLng(this.latitude, this.longhitude)
  //   }
  // }
};
</script>

<style lang="scss">
.leaflet-top.leaflet-left {
  display: none;
} 
</style>