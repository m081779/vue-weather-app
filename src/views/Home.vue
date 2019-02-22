<template>
<div>
	<div id="home" v-if="isFound">
		<div v-if="isLoading">Loading...</div>
		<div class="weather_info" v-if="!isLoading">
			<p class="error_message">{{errMessage}}</p>
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
</div>
</template>

<script>
import axios from 'axios';

export default {
	name: 'home',
	components: {
	},
	props: ['myProps'],
	data() {
		return {
			isLoading: undefined,
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
	watch: {
		myProps: function(newVal, oldVal) {
			if (newVal.searchLocation !== oldVal.searchLocation){
				this.queryLocation(newVal.searchLocation);
				newVal.searchLocation = '';
				oldVal.searchLocation = '';
			}
			if (newVal.errMessage !== oldVal.errMessage){
				this.errMessage = newVal.errMessage;

			}
		}
	},
	methods: {
		queryLocation(location) {
			console.log('locationasdfasdfas: ', location);

			this.isLoading = true;
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
					const { main, weather, wind, name } = res.data;
					this.city = name;
					this.humidity = main.humidity;
					this.pressure = main.pressure;
					this.temp = main.temp;
					this.description = weather[0].description;
					this.icon = weather[0].icon;
					this.main = weather[0].main;
					this.windSpeed = wind.speed;
					this.direction = wind.deg;
					this.isLoading = false;
					this.isFound = true;
				}).catch(() => {
					this.isLoading = false;
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
			this.errMessage = '';
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
