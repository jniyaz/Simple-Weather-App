<template>
  <div class="container" id="app">
    <h1><span class="glyphicon glyphicon-list-alt"></span> Weather App</h1>
    <div class="alert alert-info alert-dismissible" role="alert">
      <button type="button" class="close" data-dismiss="alert" aria-label="Close"><span aria-hidden="true">&times;</span></button>
      This is a simple weather app using VueJs.
    </div>
    <div class="row">
        <div class="col-md-4">
          <div class="panel panel-default">
          <div class="panel-body">
              <form v-on:submit.prevent="getWlocationDetails()">
                <div class="form-group label-floating">
                  <label for="i5" class="control-label">Weather Location</label>
                  <input type="text" required class="form-control" v-model="wLocation" id="wLocation" placeholder="Search Here..">
                  <!-- <button v-on:click="getWlocationDetails()" class="btn btn-sm btn-raised btn-primary">Search</button> -->
                  <span class="help-block">Enter your search location like <code>chennai, kuala lumpur etc..</code></span>                  
                </div>
              </form>
          </div>
        </div>          
        </div>
        <div class="col-md-8">
          <!-- <div v-if="ads" class="text-center">
            <WeatherDetails></WeatherDetails>
          </div> -->
          <div class="newslist panel" v-if="sources.current_observation">
            <ul class="media-list">
                <li class="media">
                    <div class="media-left">
                        <a target="_blank">
                          <img v-if="sources.current_observation" :src="sources.current_observation.icon_url" class="media-object">
                        </a>
                    </div>
                    <div class="media-body text-left">
                      <h4>Location</h4>
                        <span v-show="sources.location"><a :href="sources.location.wuiurl" target="_blank">{{ sources.current_observation.display_location.full }}</a></span>
                        <h4>Temperature</h4>
                        <span v-show="sources.current_observation">{{ sources.current_observation.feelslike_string }}</span>
                        <h4>Weather Status</h4>
                        <span v-show="sources.current_observation">{{ sources.current_observation.icon }}</span>
                        <br><br>
                    </div>
                </li>
            </ul>
          </div>  
          <div class="newslist panel no-data" v-else="sources.current_observation">No details available at the moment. Try again.</div>
        </div>
      </div>
  </div>
</template>

<script>
$.material.init();

import axios from 'axios'

export default {
  name: 'App',
  data () {
    return {
      ads: true,
      sources: '',
      wLocation: '',
      keyValue: '01b9b81dafc29556'
    }
  },
  mounted() {
    this.getDetails()
  },
  methods: {
    getDetails(){
        var self = this;        
        // Make a get request
        var apiUrl = 'http://api.wunderground.com/api/' + self.keyValue + '/geolookup/conditions/q/'
        var storedLocation = localStorage.getItem('storedLocation');

        if (storedLocation) {
          axios.get(apiUrl + storedLocation +'.json')
          .then(function (response) {
            // console.log(response);
            self.sources = response.data;
          })
          .catch(function (error) {
            console.log(error);
          });
        } else {
          axios.get(apiUrl + '/chennai.json')
          .then(function (response) {
            // console.log(response);
            self.sources = response.data;
          })
          .catch(function (error) {
            console.log(error);
          });
        }        
     },
    
     getWlocationDetails(){
        var self = this;
        var apiUrl = 'http://api.wunderground.com/api/' + self.keyValue + '/geolookup/conditions/q/';
        if (self.wLocation) {
          axios.get(apiUrl + self.wLocation +'.json')
          .then(function (response) {
            // console.log(response);
            self.sources = response.data;
            localStorage.setItem('storedLocation', self.wLocation);
          })
          .catch(function (error) {
            console.log(error);
          });
        }
     }
  }
}
</script>

<style>
#app {
  padding-top: 20px;
}
.media-object{
    width: 125px;
    padding: 10px;
}
.media{
    border-top: 1px solid #ccc;
    padding-top: 20px;
}
.no-data{ padding: 20px;  }
.credits{font-size: 11px; color: #ccc;margin-top: 20px;}
</style>
