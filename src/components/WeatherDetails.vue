<template>
	<div class="newslist panel">
            <ul class="media-list">
                <li class="media">
                    <div class="media-left">
                        <a target="_blank">
                        	<img :src="sources.current_observation.icon_url" class="media-object">
                        </a>
                    </div>
                    <div class="media-body text-left">
                    	<h4>Location</h4>
                        <span><a :href="sources.location.wuiurl" target="_blank">{{ sources.current_observation.display_location.full }}</a></span>
                        <h4>Temperature</h4>
                        <span>{{ sources.current_observation.feelslike_string }}</span>
                        <h4>Weather Status</h4>
                        <span>{{ sources.current_observation.icon }}</span>
                        <br><br>
                    </div>
                </li>
            </ul>
    </div>	
</template>
<script>
	import axios from 'axios'

	export default {
		name: 'weatherdetails',
		data () {
			return {
				sources: ''
			}
		},
		mounted() {
            this.getDetails()
        },
		methods: {
			getDetails(){
				var self = this;
				// Make a get request
				// http://api.wunderground.com/api/01b9b81dafc29556/geolookup/q/autoip.json
				axios.get('http://api.wunderground.com/api/01b9b81dafc29556/geolookup/conditions/q/IA/Cedar_Rapids.json')
				  .then(function (response) {
				    // console.log(response);
				    self.sources = response.data;
				  })
				  .catch(function (error) {
				    console.log(error);
				  });
			},

			submitDat(){
				alert(123);
			}
		}		
	}	
</script>
<style scoped>
    .media-object{
        width: 125px;
        padding: 10px;
    }
    .media{
        border-top: 1px solid #ccc;
        padding-top: 20px;
    }
</style>
