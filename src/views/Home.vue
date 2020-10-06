<template>
  <div>
    <LMap :zoom="zoom" :center="center" style="height: 800px; width: 80%; margin: 0 auto">
      <LTileLayer :url="url" :attribution="attribution"></LTileLayer>
      <template v-for="marker in stations">
      <LMarker :key="marker.nr" :lat-lng="convertIntoPosition(marker)" v-if="checkedVoivodeship.includes(marker.wojewodztwo) && checkedStatus.includes(marker.status) && checkedType.includes(marker.typStacji) && checkedAreaType.includes(marker.typObszaru)">
        <LPopup v-if="marker.status === 'nieaktywny'" :content="'Kod stacji: ' + marker.kodStacji + 
        '<br>Status: ' + marker.status + '<br>Typ stacji: ' + marker.typStacji + '<br>Typ obszaru: ' + marker.typObszaru + 
        '<br>Data uruchomienia: ' + marker.dataUruchomienia + '<br>Data zamknięcia: ' + marker.dataZamkniecia" ></LPopup>
        <LPopup v-if="marker.status === 'aktywny'" :content="'Kod stacji: ' + marker.kodStacji + 
        '<br>Status: ' + marker.status + '<br>Typ stacji: ' + marker.typStacji + '<br>Typ obszaru: ' + marker.typObszaru + 
        '<br>Data uruchomienia: ' + marker.dataUruchomienia" ></LPopup>
      </LMarker>
      </template>
      <LPolygon :lat-lngs="coordinates"></LPolygon>
    </Lmap>
    <input type="checkbox" id="selectAllVoivodeship" @click="selectAllVoivodeship" checked>
    <label for="selectAllVoivodeship">Wybierz wszystkie województwa</label>
    <input type="checkbox" id="dolnoslaskie" value="DOLNOŚLĄSKIE" v-model="checkedVoivodeship">
    <label for="dolnoslaskie">Dolnośląskie</label>
    <input type="checkbox" id="kujawsko-pomorskie" value= "KUJAWSKO-POMORSKIE" v-model="checkedVoivodeship">
    <label for="kujawsko-pomorskie">Kujawsko-pomorskie</label>
    <input type="checkbox" id="lubelskie" value="LUBELSKIE" v-model="checkedVoivodeship">
    <label for="lubelskie">Lubelskie</label>
    <input type="checkbox" id="lodzkie" value="ŁÓDZKIE" v-model="checkedVoivodeship">
    <label for="lodzkie">Łódzkie</label>
    <input type="checkbox" id="lubuskie" value= "LUBUSKIE" v-model="checkedVoivodeship">
    <label for="lubuskie">Lubuskie</label>
    <input type="checkbox" id="malopolskie" value= "MAŁOPOLSKIE" v-model="checkedVoivodeship">
    <label for="malopolskie">Małopolskie</label>
    <input type="checkbox" id="mazowieckie" value= "MAZOWIECKIE" v-model="checkedVoivodeship">
    <label for="mazowieckie">Mazowieckie</label>
    <input type="checkbox" id="opolskie" value= "OPOLSKIE" v-model="checkedVoivodeship">
    <label for="opolskie">Opolskie</label>
    <input type="checkbox" id="podlaskie" value= "PODLASKIE" v-model="checkedVoivodeship">
    <label for="podlaskie">Podlaskie</label>
    <input type="checkbox" id="podkarpackie" value= "PODKARPACKIE" v-model="checkedVoivodeship">
    <label for="podkarpackie">Podkarpackie</label>
    <input type="checkbox" id="pomorskie" value= "POMORSKIE" v-model="checkedVoivodeship">
    <label for="pomorskie">Pomorskie</label>
    <input type="checkbox" id="swietokrzyskie" value= "ŚWIĘTOKRZYSKIE" v-model="checkedVoivodeship">
    <label for="swietokrzyskie">Świętokrzyskie</label>
    <input type="checkbox" id="slaskie" value= "ŚLĄSKIE" v-model="checkedVoivodeship">
    <label for="slaskie">Śląskie</label>
    <input type="checkbox" id="warminsko-mazurskie" value= "WARMIŃSKO-MAZURSKIE" v-model="checkedVoivodeship">
    <label for="warminsko-mazurskie">Warmińsko-mazurskie</label>
    <input type="checkbox" id="wielkopolskie" value= "WIELKOPOLSKIE" v-model="checkedVoivodeship">
    <label for="wielkopolskie">Wielkopolskie</label>
    <input type="checkbox" id="zachodniopomorskie" value= "ZACHODNIOPOMORSKIE" v-model="checkedVoivodeship">
    <label for="zachodniopomorskie">Zachodniopomorskie</label>

    <input type="checkbox" id="selectAllStatus" @click="selectAllStatus" checked>
    <label for="selectAllStatus">Wybierz status stacji</label>
    <input type="checkbox" id="aktywny" value="aktywny" v-model="checkedStatus">
    <label for="aktywny">Aktywny</label>
    <input type="checkbox" id="nieaktywny" value="nieaktywny" v-model="checkedStatus">
    <label for="nieaktywny">Nieaktywny</label>

    <input type="checkbox" id="selectAllType" @click="selectAllType" checked>
    <label for="selectAllType">Wybierz typ stacji</label>
    <input type="checkbox" id="przemyslowa" value="przemysłowa" v-model="checkedType">
    <label for="przemyslowa">Przemysłowa</label>
    <input type="checkbox" id="tlo" value="tło" v-model="checkedType">
    <label for="tlo">Tło</label>
    <input type="checkbox" id="komunikacyjna" value="komunikacyjna" v-model="checkedType">
    <label for="komunikacyjna">Komunikacyjna</label>

    <input type="checkbox" id="selectAllAreaType" @click="selectAllAreaType" checked>
    <label for="selectAllAreaType">Wybierz rodzaj obszaru stacji</label>
    <input type="checkbox" id="podmiejski" value="podmiejski" v-model="checkedAreaType">
    <label for="podmiejski">Podmiejski</label>
    <input type="checkbox" id="miejski" value="miejski" v-model="checkedAreaType">
    <label for="miejski">Miejski</label>
    <input type="checkbox" id="pozamiejski" value="pozamiejski" v-model="checkedAreaType">
    <label for="pozamiejski">Pozamiejski</label>
  </div>
</template>

<script>
// @ is an alias to /src
import L from 'leaflet';
import { LMap, LTileLayer, LMarker, LPopup, LPolygon} from 'vue2-leaflet';
import data from '../data/json_meta.json';

export default {
  name: 'Home',
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
    LPolygon
  },
  data() {
    return {
      zoom: 6,
      center: L.latLng(52.505005, 18.809358),
      url:'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
      attribution:'&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      data: data,
      display: true,
      stations: data,
      checkedVoivodeship: ['DOLNOŚLĄSKIE','KUJAWSKO-POMORSKIE','LUBELSKIE','ŁÓDZKIE','LUBUSKIE','MAŁOPOLSKIE','ZACHODNIOPOMORSKIE','WIELKOPOLSKIE','WARMIŃSKO-MAZURSKIE','ŚLĄSKIE','MAZOWIECKIE','OPOLSKIE','PODLASKIE','PODKARPACKIE','POMORSKIE','ŚWIĘTOKRZYSKIE'],
      checkedStatus: ['aktywny','nieaktywny'],
      checkedType: ['przemysłowa','tło','komunikacyjna'],
      checkedAreaType: ['podmiejski','miejski','pozamiejski'],
      coordinates: [ [51.205305155694361, 16.906177973598364 ], [51.204564937448588, 16.906936631095761 ], [52.204121280948854, 16.907685529979776 ], [52.203973445510087, 17.90793100864386 ]]
    }
  },
  watch: {
    checkedVoivodeship: function() {
      const checkbox = document.getElementById('selectAllVoivodeship');
      if(this.checkedVoivodeship.length < 16){
        return checkbox.checked = false;
      }else{
        return checkbox.checked = true;
      }
    },
    checkedStatus: function() {
      const checkbox = document.getElementById('selectAllStatus');
      if(this.checkedStatus.length < 2){
        return checkbox.checked = false;
      }else{
        return checkbox.checked = true;
      }
    },
    checkedType: function() {
      const checkbox = document.getElementById('selectAllType');
      if(this.checkedType.length < 3){
        return checkbox.checked = false;
      }else{
        return checkbox.checked = true;
      }
    },
    checkedAreaType: function() {
      const checkbox = document.getElementById('selectAllAreaType');
      if(this.checkedAreaType.length < 3){
        return checkbox.checked = false;
      }else{
        return checkbox.checked = true;
      }
    }
  },
  methods: {
    convertIntoPosition(station){
      const position = L.latLng(station.lat, station.lon);
      return position
    },
    selectAllVoivodeship(){
      if(this.checkedVoivodeship.length === 16){
        this.checkedVoivodeship = [];
        return
      }
      this.checkedVoivodeship = ['DOLNOŚLĄSKIE','KUJAWSKO-POMORSKIE','LUBELSKIE','ŁÓDZKIE','LUBUSKIE','MAŁOPOLSKIE','ZACHODNIOPOMORSKIE','WIELKOPOLSKIE','WARMIŃSKO-MAZURSKIE','ŚLĄSKIE','MAZOWIECKIE','OPOLSKIE','PODLASKIE','PODKARPACKIE','POMORSKIE','ŚWIĘTOKRZYSKIE'];
    },
    selectAllStatus(){
      if(this.checkedStatus.length === 2){
        this.checkedStatus = [];
        return
      }
      this.checkedStatus = ['aktywny','nieaktywny'];
    },
    selectAllType(){
      if(this.checkedType.length === 3){
        this.checkedType = [];
        return
      }
      this.checkedType = ['przemysłowa','tło','komunikacyjna'];
    },
    selectAllAreaType(){
      if(this.checkedAreaType.length === 3){
        this.checkedAreaType = [];
        return
      }
      this.checkedAreaType = ['podmiejski','miejski','pozamiejski'];
    }
  }
}
</script>

<style lang="scss" scoped>

</style>