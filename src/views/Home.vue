<template>
  <div> 
    <div class="mapBox">
      <LMap :zoom="zoom" :center="center" class="mapBox--mapStyle">
        <LTileLayer :url="url" :attribution="attribution"></LTileLayer>
        <template v-for="marker in stations">
          <LMarker :key="marker.nr" :lat-lng="convertIntoPosition(marker)" v-if="checkedVoivodeship.includes(marker.wojewodztwo) && checkedStatus.includes(marker.status) && checkedType.includes(marker.typStacji) && checkedAreaType.includes(marker.typObszaru)">
            <LIcon>
              <svg v-if="marker.typStacji === 'tło'" :class="{'red':marker.typObszaru === 'miejski','orange':marker.typObszaru === 'podmiejski','yellow':marker.typObszaru === 'pozamiejski' }">
                <use href="#background"/>  
              </svg>
              <svg v-else-if="marker.typStacji === 'przemysłowa'" :class="{'red':marker.typObszaru === 'miejski','orange':marker.typObszaru === 'podmiejski','yellow':marker.typObszaru === 'pozamiejski' }">
                <use href="#industry"/>  
              </svg>
              <svg v-else-if="marker.typStacji === 'komunikacyjna'" :class="{'red':marker.typObszaru === 'miejski','orange':marker.typObszaru === 'podmiejski','yellow':marker.typObszaru === 'pozamiejski' }">
                <use href="#communication"/>
              </svg>
            </LIcon>
            <Popup :info="marker"></Popup>
          </LMarker>
        </template>
        <!--LPolygon :lat-lngs="coordinates"></LPolygon-->
      </Lmap>
      <ul class="mainList">
        <li>
          <h3>Wybierz województwa</h3>
          <ul>
            <li>
              <input type="checkbox" id="selectAllVoivodeship" @click="selectAllVoivodeship" checked>
              <label for="selectAllVoivodeship">
                <h5>Zaznacz/Odznacz wszystkie</h5>
              </label>
            </li>
            <li>
              <input type="checkbox" id="dolnoslaskie" value="DOLNOŚLĄSKIE" v-model="checkedVoivodeship">
              <label for="dolnoslaskie">
                <h5>Dolnośląskie</h5>
              </label>
            </li>
            <li>
              <input type="checkbox" id="kujawsko-pomorskie" value= "KUJAWSKO-POMORSKIE" v-model="checkedVoivodeship">
              <label for="kujawsko-pomorskie">
                <h5>Kujawsko-pomorskie</h5>
              </label>
            </li>
            <li>
              <input type="checkbox" id="lubelskie" value="LUBELSKIE" v-model="checkedVoivodeship">
              <label for="lubelskie">
                <h5>Lubelskie</h5>
              </label>
            </li>
            <li>
              <input type="checkbox" id="lodzkie" value="ŁÓDZKIE" v-model="checkedVoivodeship">
              <label for="lodzkie">
                <h5>Łódzkie</h5>
              </label>
            </li>
            <li>
              <input type="checkbox" id="lubuskie" value= "LUBUSKIE" v-model="checkedVoivodeship">
              <label for="lubuskie">
                <h5>Lubuskie</h5>
              </label>
            </li>
            <li>
              <input type="checkbox" id="malopolskie" value= "MAŁOPOLSKIE" v-model="checkedVoivodeship">
              <label for="malopolskie">
                <h5>Małopolskie</h5>
              </label>
            </li>
            <li>
              <input type="checkbox" id="mazowieckie" value= "MAZOWIECKIE" v-model="checkedVoivodeship">
              <label for="mazowieckie">
                <h5>Mazowieckie</h5>
              </label>
            </li>
            <li>
              <input type="checkbox" id="opolskie" value= "OPOLSKIE" v-model="checkedVoivodeship">
              <label for="opolskie">
                <h5>Opolskie</h5>
              </label>
            </li>
            <li>
              <input type="checkbox" id="podlaskie" value= "PODLASKIE" v-model="checkedVoivodeship">
              <label for="podlaskie">
                <h5>Podlaskie</h5>
              </label>
            </li>
            <li>
              <input type="checkbox" id="podkarpackie" value= "PODKARPACKIE" v-model="checkedVoivodeship">
              <label for="podkarpackie">
                <h5>Podkarpackie</h5>
              </label>
            </li>
            <li>
              <input type="checkbox" id="pomorskie" value= "POMORSKIE" v-model="checkedVoivodeship">
              <label for="pomorskie">
                <h5>Pomorskie</h5>
              </label>
            </li>
            <li>
              <input type="checkbox" id="swietokrzyskie" value= "ŚWIĘTOKRZYSKIE" v-model="checkedVoivodeship">
              <label for="swietokrzyskie">
                <h5>Świętokrzyskie</h5>
              </label>
            </li>
            <li>
              <input type="checkbox" id="slaskie" value= "ŚLĄSKIE" v-model="checkedVoivodeship">
              <label for="slaskie">
                <h5>Śląskie</h5>
              </label>
            </li>
            <li>
              <input type="checkbox" id="warminsko-mazurskie" value= "WARMIŃSKO-MAZURSKIE" v-model="checkedVoivodeship">
              <label for="warminsko-mazurskie">
                <h5>Warmińsko-mazurskie</h5>
              </label>
            </li>
            <li>
              <input type="checkbox" id="wielkopolskie" value= "WIELKOPOLSKIE" v-model="checkedVoivodeship">
              <label for="wielkopolskie">
                <h5>Wielkopolskie</h5>
              </label>
            </li>
            <li>
              <input type="checkbox" id="zachodniopomorskie" value= "ZACHODNIOPOMORSKIE" v-model="checkedVoivodeship">
              <label for="zachodniopomorskie">
                <h5>Zachodniopomorskie</h5>
              </label>
            </li>
          </ul>
        </li>
        <li>
          <h3>Wybierz status stacji</h3>
          <ul>
            <li>
              <input type="checkbox" id="selectAllStatus" @click="selectAllStatus" checked>
              <label for="selectAllStatus">
                <h5>Zaznacz/odznacz wszystkie</h5>
              </label>
            </li>
            <li>
              <input type="checkbox" id="aktywny" value="aktywny" v-model="checkedStatus">
              <label for="aktywny">
                <h5>Aktywny</h5>
              </label>
            </li>
            <li>
              <input type="checkbox" id="nieaktywny" value="nieaktywny" v-model="checkedStatus">
              <label for="nieaktywny">
                <h5>Nieaktywny</h5>
              </label>
            </li>
          </ul>
        </li>
        <li>
          <h3>Wybierz typ stacji</h3>
          <ul>
            <li>
              <input type="checkbox" id="selectAllType" @click="selectAllType" checked>
              <label for="selectAllType">
                <h5>Zaznacz/odznacz wszystkie</h5>
              </label>
            </li>
            <li>
              <input type="checkbox" id="przemyslowa" value="przemysłowa" v-model="checkedType">
              <label for="przemyslowa">
                <h5>Przemysłowa</h5>
                <svg>
                  <use href="#industry"/>  
                </svg>
              </label>
            </li>
            <li>
              <input type="checkbox" id="tlo" value="tło" v-model="checkedType">
              <label for="tlo">
                <h5>Tło</h5>
                <svg>
                  <use href="#background"/>  
                </svg>
              </label>
            </li>
            <li>
              <input type="checkbox" id="komunikacyjna" value="komunikacyjna" v-model="checkedType">
              <label for="komunikacyjna">
                Komunikacyjna
                <svg>
                  <use href="#communication"/>  
                </svg>
              </label>
            </li>
          </ul>
        </li>
        <li>
          <h3>Wybierz rodzaj obszaru stacji</h3>
          <ul>
            <li>
              <input type="checkbox" id="selectAllAreaType" @click="selectAllAreaType" checked>
              <label for="selectAllAreaType">
                <h5>Zaznacz/odznacz wszystkie</h5>
              </label>
            </li>
            <li>
              <input type="checkbox" id="podmiejski" value="podmiejski" v-model="checkedAreaType">
              <label for="podmiejski">
                <h5>Podmiejski</h5>
                <div class="orange-div"></div>
              </label>
            </li>
            <li>
              <input type="checkbox" id="miejski" value="miejski" v-model="checkedAreaType">
              <label for="miejski">
                <h5>Miejski</h5>
                <div class="red-div"></div>
              </label>
            </li>
            <li>
              <input type="checkbox" id="pozamiejski" value="pozamiejski" v-model="checkedAreaType">
              <label for="pozamiejski">
                <h5>Pozamiejski</h5>
                <div class="yellow-div"></div>
              </label>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import L from 'leaflet';
import { LMap, LTileLayer, LMarker, LPopup, LPolygon, LIcon} from 'vue2-leaflet';
import data from '../data/json_meta.json';
import pm10 from '../data/PM10_statsy.json';
import Popup from '../components/Popup.vue';

export default {
  name: 'Home',
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
    LPolygon,
    LIcon,
    Popup
  },
  data() {
    return {
      zoom: 6,
      center: L.latLng(52.505005, 18.809358),
      url:'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
      attribution:'&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      data: data,
      pm10: pm10,
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
    },
  },
  methods: {
    convertIntoPosition(station){
      const position = L.latLng(station.lat, station.lon);
      return position
    },
    selectAllVoivodeship(){
      console.log(this.pm10.PM10[0].Województwo);
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
    },
  }
}
</script>

<style lang="scss" scoped>

  $colorPrimary: #000501;
  $colorSecondary: #73ab84;
  $colorThird: #99d19c;
  $colorBackground: #f7f0f5;

  h3, h5 {
    color: $colorPrimary;
  }

  .mapBox {
    width: 100vw;
    height: 100vh;

    &--mapStyle {
      width: 70%;
      height: 80%;
      margin: 2.5%;
    }
  }

  .mainList {
    width: 20%;
    height: 80%;
    margin: 2.5%;
    padding-left: 10px;
    box-sizing: border-box;
    overflow: auto;
  }

  .mainList h3{
    padding: 10px 0;
    font-size: 24px;
  }

  .mainList label{
    display: flex;
    padding-left: 30px;
    align-items: center;
    font-size: 16px;
  }

  .mainList label svg{
    margin-left: 10px;
    height: 20px;
    width: 20px;
  }

  //dodać klasę zamiast li
  li {
    margin: 10px;
  }

  input[type="checkbox"] {
    display: none;
  }

  input[type="checkbox"] + label {
    position: relative;
    cursor: pointer;
    text-justify: center;
  }

  [type="checkbox"]:not(:checked) + label:before,
  [type="checkbox"]:checked + label:before {
    content: '';
    position: absolute;
    left: 0;
    top: -4px;
    width: 17px;
    height: 17px;
    border: 3px solid white;
    border-radius: 5px;
    box-shadow: 0 0 0 1px $colorSecondary;
    margin-right: 5px;
  }

  [type="checkbox"]:checked + label:before {
    background-color: $colorThird;
    border-color: white;
    outline-color: $colorThird;
  }

  .red{
    fill: red;
    height: 20px;
    width: 20px;
  }

  .orange{
    fill: orange;
    height: 20px;
    width: 20px;
  }

  .yellow{
    fill: yellow;
    height: 20px;
    width: 20px;
  }

  .red-div{
    height: 20px;
    width: 20px;
    background-color: red;
    margin-left: 10px;
  }

  .orange-div{
    height: 20px;
    width: 20px;
    background-color: orange;
    margin-left: 10px;
  }

  .yellow-div{
    height: 20px;
    width: 20px;
    background-color: yellow;
    margin-left: 10px;
  }

  @media (min-width: 1080px) {
  .mapBox {
    display: flex;
    &--mapStyle {
      width: 70%;
      height: 80%;
      margin: 2.5%;
    }
  }
}
</style>