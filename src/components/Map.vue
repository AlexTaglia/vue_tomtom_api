<template>
  <div class="container">
    <div class="row text-center mt-3 mb-3">
      <div class="col-12">
        <!-- <form action=""> -->
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


        <!-- </form> -->
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
    }
  },

  mounted() {
    this.getMap()
    // var map = this.tt.map({
    //   key: 'K35TCsKGW4huvzCAhYmRLYjgHewqTyhe',
    //   container: 'map',
    //   center: [this.lat, this.long],
    //   zoom: 15,
    // });
    // map.addControl(new this.tt.FullscreenControl());
    // map.addControl(new this.tt.NavigationControl());
    // this.addMarker(map);
  },

  methods:{
    getMap(){
    var map = this.tt.map({
      key: 'K35TCsKGW4huvzCAhYmRLYjgHewqTyhe',
      container: 'map',
      center: [this.long, this.lat],
      zoom: 15,
    });
    map.addControl(new this.tt.FullscreenControl());
    map.addControl(new this.tt.NavigationControl());
    this.addMarker(map);
    },

    addMarker(map){
      var location = [this.long, this.lat];
      var popupOffsets = {
        top: [0, 0],
        bottom: [0, -30],
        'bottom-right': [0, -30],
        'bottom-left': [0, -30],
        left: [25, -35],
        right: [-25, -35]
      } 

      var marker = new this.tt.Marker().setLngLat(location).addTo(map);
      var popup = new this.tt.Popup({offset: popupOffsets}).setHTML("ecco!");
      marker.setPopup(popup).togglePopup();
    }

  }


}
</script>

<style lang="scss">


#map {
    height: 80vh;
    width: 100%;
    border: 1px solid #d3d1d1;
    border-radius: 10px;
    box-shadow: 10px 10px 20px #d3d1d1;
}

input{
  margin-right: 20px;
}


</style>