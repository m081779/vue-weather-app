<template>
  <div id="home">
    <LocationSearch v-on:search-location="searchLocation" v-on:error="addError"/>
	<p class="error_message">{{errMessage}}</p>
	<div class="weather_info" v-if="isFound">
		<p>City: {{city}}</p>
		<p>Humidity: {{humidity}}</p>
		<p>Pressure: {{pressure}}</p>
		<p>Temp: {{temp}}</p>
		<p>Description: {{description}}</p>
		<p>Icon: {{icon}}</p>
		<p>Main: {{main}}</p>
		<p>Wind Speed: {{windSpeed}}</p>
		<p>Direction: {{direction}}</p>
	</div>
  </div>
</template>

<script>
import axios from 'axios';
import LocationSearch from '@/components/LocationSearch.vue'

export default {
	name: 'home',
	components: {
		LocationSearch
	},
	data() {
		return {
			isFound: false,
			city: '',
			humidity: '',
			pressure: '',
			temp: '',
			description: '',
			icon: '',
			main: '',
			windSpeed: '',
			direction: '',
			errMessage: ''
		}
	},
	methods: {
		searchLocation(location) {
			this.clearResults();
			let baseUrl = `https://api.openweathermap.org/data/2.5/weather?&appid=efab3183191d888ebe50a33684084a10&`
			const testForZipcode = /^\d{5}(?:[-\s]\d{4})?$/;
			if (testForZipcode.test(location)){
				baseUrl += `zip=${location}`
			} else {
				baseUrl += `q=${location}`
			}
			if(location) {
				axios
				.get(baseUrl)
				.then(res => {
					// console.log('res: ', res);
					const { main, weather, wind, name } = res.data;
					this.isFound = true;
					this.city = name;
					this.humidity = main.humidity;
					this.pressure = main.pressure;
					this.temp = main.temp;
					this.description = weather[0].description;
					this.icon = weather[0].icon;
					this.main = weather[0].main;
					this.windSpeed = wind.speed;
					this.direction = wind.deg;
				}).catch(() => {
					this.isFound = false;
					this.errMessage = 'No location found, please try again.'
				});
			}
		},
		addError(errMessage) {
			this.errMessage = errMessage;
		},
		clearResults() {
			this.isFound = false;
			this.city = '';
			this.humidity = '';
			this.pressure = '';
			this.temp = '';
			this.description = '';
			this.icon = '';
			this.main = '';
			this.windSpeed = '';
			this.direction = '';
		}
	}
}
</script>

<style scoped>
#home {
	background: #ddd;
	border-radius: 5px;
	padding: 20px;
	border: 1px solid #777;
	min-height: 300px;
	width: 50%;
	margin: 50px auto;
}
.error_message {
	color: red;
}
</style>
