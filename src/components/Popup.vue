<template>
  <div>
    <LPopup :content="content"></LPopup>
  </div>
</template>

<script>

import {LPopup} from 'vue2-leaflet';
import PM10 from '../data/PM10_statsy.json';

export default {
  name: 'Popup',
  components: {
    LPopup,
  },
  props: {
    info: Object
  },
  computed: {
    content: function(){
      let messeage = 'Kod stacji: ' + this.info.kodStacji + '<br>Status: ' + this.info.status + '<br>Typ stacji: ' + this.info.typStacji + '<br>Typ obszaru: ' + this.info.typObszaru + '<br>Data uruchomienia: ' + this.info.dataUruchomienia;
      if(this.info.hasOwnProperty('dataZamkniecia')){
        messeage += '<br>Data zamknięcia: ' + this.info.dataZamkniecia;
      }
      // za bardzo obciąża
      /*if(this.pm10.PM10.filter(x => x.kodStacji === this.info.kodStacji)){
        messeage += '<br>PM10';
      }*/
      return messeage
    }
  },
  data(){
    return {
      pm10: PM10,
    }
  },
  methods: {
    
  }
}
</script>

<style lang="scss" scoped>

</style>
