<template>
    <header>
			<div class="weather">Weather App</div>
            <LocationSearch v-on:search-location="setSearchLocation" v-on:error="addError"/>
			<ul id="nav">
				<li><router-link to="/">Home</router-link></li> 
				<li>|</li>
				<li><router-link to="/five-day">FiveDay</router-link></li>
			</ul>
		</header>
</template>

<script>
import LocationSearch from './LocationSearch'
export default {
    name: 'Header',
    components: {
        LocationSearch
    },
    methods: {
        addError(error) {
            this.$emit('add-error', error);
        }, 
        setSearchLocation(searchLocation){
            this.$emit('search-location', searchLocation);
        }
    }
}
</script>

