<template>
	<form class="location_search_form" @submit="submitSearch">
		<input class="location_input" type="text" name="location" placeholder="Enter City or Zipcode..." v-model="location">
		<input class="submit_button" type="submit" value="Submit">
	</form>
</template>

<script>
export default {
	name: 'LocationSearch',
	data() {
		return {
			location: ''
		}
	},
	methods: {
		submitSearch(e) {
			e.preventDefault();
			const testForZipcode = /^\d{5}(?:[-\s]\d{4})?$/;
			const testForText = /^[a-zA-Z\s]*$/
			const location = this.location.trim();
			if (location && (testForZipcode.test(location) || testForText.test(location))){
				this.$emit('search-location', this.location);
				this.$emit('error', '');
			} else {
				this.$emit('error', 'Please enter a valid location');
			}
			this.location = '';
		}
	}
}
</script>

<style scoped>
.submit_button {
	height: 30px;
	padding: 0px 10px;
	background: slateblue;
	color: white;
	outline: none;
	border: 1px solid navy;
	font-size: 16px;
	border-bottom-right-radius: 5px; 
	border-top-right-radius: 5px; 
}
.location_input {
	height: 30px;
	padding: 0px 10px;
	width: 30%;
	border-bottom-left-radius: 5px; 
	border-top-left-radius: 5px; 
}
</style>
