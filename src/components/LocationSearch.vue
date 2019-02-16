<template>
	<form class="location_search_form" @submit="submitSearch">
		<input type="text" name="location" placeholder="Enter City or Zipcode..." v-model="location">
		<input type="submit" value="submit">
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
			console.log()
			if (location && (testForZipcode.test(location) || testForText.test(location))){
				console.log('firing');
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

</style>
