<template>
  <div class="ui grid">
    <div class="six wide column">
      <form class="ui segment large form">
        <div class="ui segment">
          <div class="field">
            <div class="ui right icon input large">
              <input type="text" placeholder="Enter your address" v-model="coordinates" />
              <i class="dot circle link icon" @click="locatorButtonPressed"></i>
            </div>
          </div>
          <div class="field">
            <div class="two fields">
              <div class="field">
                <input type="text" placeholder="Enter Radius Km" v-model="radius" />
              </div>
            </div>
          </div>
          <div class="ui primary button" tabindex="0" @click="findCloseBuyButtonPressed">
            Search
          </div>
        </div>
      </form>
      <div class="ui segment" style="max-height:500px;overflow:scroll">
        <div class="ui divided items">
          <div class="item" v-for="place in places" :key="place.id">
            <div class="content">
              <div class="header">{{ place.name }}</div>
              <div class="meta">{{ place.vicinity }}</div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="ten wide column segment ui" ref="map"></div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: 'HelloWorld',
  data() {
    return {
      lat: 0,
      lng: 0,
      radius: "",
      places: []
    };
  },
  computed: {
    coordinates() {
      return `${this.lat}, ${this.lng}`;
    }
  },
  methods: {
    locatorButtonPressed() {
      navigator.geolocation.getCurrentPosition(
        position => {
          this.lat = position.coords.latitude;
          this.lng = position.coords.longitude;
        }
      );
    },
    findCloseBuyButtonPressed() {
      console.log("burda");
      const URL = `https://google-map-spring-boot.herokuapp.com/nearby-search/${this.lat}/${this.lng}/${this.radius * 1000}`;
      axios.get(URL).then(response => {
        this.places = response.data.data;
        console.log("sdfds " + response.data);
        this.initMap();
      }).catch(error => {
        console.log(error.message);
      });
    },
    initMap() {
      var map = new window.google.maps.Map(this.$refs['map'], {
        zoom: 15,
        center: new window.google.maps.LatLng(this.lat, this.lng),
        mapTypeId: window.google.maps.MapTypeId.ROADMAP
      });
      var infowindow = new window.google.maps.InfoWindow();
      this.places.forEach((place) => {
        const lat = place.lat;
        const lng = place.lng;
        let marker = new window.google.maps.Marker({
          position: new window.google.maps.LatLng(lat, lng),
          map: map
        });
        window.google.maps.event.addListener(marker, "click", () => {
          infowindow.setContent(`<div class="ui header">${place.name}</div><p>${place.vicinity}</p>`);
          infowindow.open(map, marker);
        });
      });
    }

  }
}
</script>
