<template>
  <div class="container">
    <div class="row text-center mt-3 ">
      <div class="col-12">
          <div class="row">
            
            <!-- Longitudine -->
            <div class="col-5">
              <div class="input-group mb-3">
                <span class="input-group-text" id="longitude">Longitudine</span>
                <input type="text" class="form-control" id="longitude" aria-describedby="longitude" v-model="long">
              </div>
            </div>

            <!-- Latitudine -->
            <div class="col-5">
              <div class="input-group mb-3">
                <span class="input-group-text" id="latitude">Latitudine</span>
                <input type="text" class="form-control" id="latitude" aria-describedby="latitude" v-model="lat" >
              </div>           
            </div>

            <!-- Cerca -->
            <div class="col-2">
              <div class="input-group mb-3">
                <button class="btn btn-primary" @click="getMap">Cerca</button>
              </div> 
            </div>

          </div>

      </div>
    </div>

    <div class="row mb-3">
      <div class="col-12">
            <div class="" id="adress"></div>
      </div>
    </div>
    <div class="row text-center">
      <div class="col-12">
        <div id='map' class='map'></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {

  data(){
    return{
      tt: window.tt,
      long: 9.191383,
      lat: 45.464211,
      key: '',
    }
  },

  mounted() {
    this.getMap()
  },

  methods:{
    // recuperare mappa con tom tom api
    getMap(){
    var map = this.tt.map({
      key: this.key,
      container: 'map',
      center: [this.long, this.lat],
      zoom: 16,
    });
    map.addControl(new this.tt.FullscreenControl());
    map.addControl(new this.tt.NavigationControl());
    this.addMarker(map);
    },

    // Aggiunge segna posto sulle coordinata
    addMarker(map){
      var location = [this.long, this.lat];
      var popupOffsets = 25;

      var marker = new this.tt.Marker().setLngLat(location).addTo(map);
      var popup = new this.tt.Popup({offset: popupOffsets});
      this.reverseGeocoding(marker, popup);
      marker.setPopup(popup).togglePopup();
    },

    // Trasforma  longitudine e latitudine in indirizzo
    reverseGeocoding(marker, popup) { 
      this.tt.services.reverseGeocode({ 
          key: this.key, 
          position: marker.getLngLat() 
      }).then( function( result ){ 
          popup.setHTML(result.addresses[0].address.freeformAddress); 

          // Aggiunge in un div l'indirizzo
          var myDiv = document.getElementById("adress");
          myDiv.innerHTML = result.addresses[0].address.freeformAddress;
      }) 
    },
    
  }


}
</script>

<style lang="scss">


#map {
    height: 50vh;
    width: 100%;
    border: 1px solid #d3d1d1;
    border-radius: 10px;
    box-shadow: 10px 10px 20px #d3d1d1;
}

input{
  margin-right: 20px;
}

#adress{
  font-weight: bold;
}


</style>