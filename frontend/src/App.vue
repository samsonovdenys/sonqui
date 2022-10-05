<template>
  <div>
      Latitude: {{currPos.lat.toFixed(7)}}, Longitude: {{currPos.lng.toFixed(7)}}
      <h1>My friends</h1>
      <AddUser 
        @add-user="addUser"
      />  
    <hr>
    <Sonqui 
      v-bind:users="users"
      v-on:remove-user="removeUser"
    />
    <hr>     
  </div>
  <div ref="mapDiv" style="width: 100%; height: 80vh" />
</template>

<script>
/* eslint-disable no-undef */
import {computed, ref, onMounted} from 'vue'
import {useGeolocation} from './useGeolocation'
import {Loader} from '@googlemaps/js-api-loader'
import Sonqui from './components/Sonqui.vue'
import AddUser from './components/AddUser.vue'

//import { faBus } from "@fortawesome/free-solid-svg-icons"

const GOOGLE_MAPS_API_KEY = 'AIzaSyAV8XG6TCh2BdEj6sg2WuXkuwxvGRMp_Nc'

  export default {
    name: "App",
    //Function data(), is here to save the data. Returns an Object with key-values wich we can take by using 'v-bind:items="key" in <template>'
    data(){
      return {
        users: [
          {id:1, nick: 'Programist', status: 'want_go_out', active: false},
          {id:2, nick: 'Comercialist', status: 'im_out', active: false},
          {id:3, nick: 'Frontender', status: 'stay_home', active: false}
        ]
      }
    },
    // method mounted() starts when HTML is ready
    mounted(){
      fetch('https://jsonplaceholder.typicode.com/todos?_limit=5')
        .then(response => response.json())
        .then(json => {
          this.users = json
        })
    },
    methods: {
      removeUser(id) {
        this.users = this.users.filter(t => t.id !== id)
      },
      addUser(user){
        this.users.push(user)
      }
    },
    setup() {
        const { coords } = useGeolocation();
        const currPos = computed(() => ({
            //lat: coords.value.latitude,
            //lng: coords.value.longitude
            lat: 45.434,
            lng: 8.6259
        }));
        const friendsPos = computed(() => ({
            lat: 45.6955546,
            lng: 8.4682284
        }));
        const loader = new Loader({ apiKey: GOOGLE_MAPS_API_KEY });
        const mapDiv = ref(null);
        onMounted(async () => {
            await loader.load();
            const map = new google.maps.Map(mapDiv.value, {
                center: currPos.value,
                zoom: 9
            });
            const image = "https://developers.google.com/maps/documentation/javascript/examples/full/images/beachflag.png";
            new google.maps.Marker({
                position: currPos.value,
                icon: {
                    path: google.maps.SymbolPath.BACKWARD_CLOSED_ARROW,
                    fillColor: "red",
                    fillOpacity: 0.7,
                    strokeWeight: 0.7,
                    scale: 4,
                },
                //label: 'D',
                map: map,
            });
            new google.maps.Marker({
                position: friendsPos.value,
                //icon: image,
                label: "2",
                map: map,
            });
            // Sets the map on all markers in the array.
            function setMapOnAll(map = null) {
                for (let i = 0; i < markers.length; i++) {
                    markers[i].setMap(map);
                }
            }
            //let mapData = 3
            //for (var i = 0; i = mapData.length; i++) {
            //        createMarker(i+1, map, mapData[i]); 
            //        extendBounds(bounds, mapData[i]);
            //}
        });
        return { currPos, mapDiv };
    },
    components: { Sonqui, AddUser}
};
</script>



<style>
#app {
  text-align: center;
}
.mapIconLabel {
    font-size: 15px;
    font-weight: bold;
    color: #FFFFFF;
    font-family: 'DINNextRoundedLTProMediumRegular';
}

</style>