<template>
  <div class="app__bg">
    <h1 class="app__header">IP Address Tracker</h1>
    <InputField  />
    <IpDetails />
  </div>
  <Map />
  <div class="attribution">
    Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank">Frontend Mentor</a>. 
    Coded by <a href="https://github.com/jastenn">Justine Aviso</a>.
  </div>
</template>

<script>
import { onMounted, provide, readonly, ref, watch } from '@vue/runtime-core'
//import getData from './composables/getData'
import InputField from './components/InputField.vue'
import IpDetails from './components/IpDetails.vue'
import Map from './components/Map.vue'

export default {
  name: 'App',
  components: {
    InputField,
    IpDetails,
    Map
  },
  setup() {
    require('dotenv').config()

    const geoLoc = ref(null)
    const loading = ref(true)
    const curIP = ref(null)
    const error = ref(false)

    onMounted(() => {
      error.value = false
      loading.value = true
      fetch(`https://geo.ipify.org/api/v1?apiKey=${process.env.VUE_APP_GEO_API_KEY}`)
        .then(res => res.json())
        .then(data => {
          geoLoc.value = data
          loading.value = false
        })
        .catch(err => {
          loading.value = false
          console.log(err)
          error.value = true
        })
    })
    
    watch(curIP, () => {
      error.value = false
      loading.value = true
      fetch(`https://geo.ipify.org/api/v1?apiKey=${process.env.VUE_APP_GEO_API_KEY}&ipAddress=${curIP.value}`)
        .then(res => res.json())
        .then(data => {
          console.log(data)
          geoLoc.value = data
          loading.value = false
        })
        .catch(err => {
          console.log(err)
          loading.value = false
          error.value = true
        })
    })

    const updateCurIP = (newIp) => {
      curIP.value = newIp
    }

    

    provide('geoLoc', readonly(geoLoc))
    provide('loading', readonly(loading))
    provide('error', readonly(error))
    provide('updateCurIP', updateCurIP)

    return {
    }
  }
}
</script>

<style>

.app__bg {
  background: #5060C0 url('./assets/img/pattern-bg.png') center;
  background-size: cover;
  height: 35vh;
  min-height: 17rem;
  padding-top: 2rem;
  position: relative;
}
.app__header {
  margin-bottom: 2rem;
  font-size: 1.65rem;
  font-size: clamp(1.63rem, 5vw, 2rem);
  font-weight: 500;
  color: white;
  text-align: center;
}
.attribution { 
  font-size: 11px; 
  text-align: center;
  background-color: white;
  padding: .3em .3em .2em .2em;
  position: absolute;
  bottom: 0;
  z-index: 2000; 
  }
.attribution a { 
  color: rgb(0, 146, 204);
  text-decoration: none;
}
</style>
