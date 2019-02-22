<template>
	<div id="app">
		<Header v-on:add-error="addError" v-on:search-location="setSearchLocation"/>
		<router-view  :myProps="myProps"/>
	</div>
</template>

<script>
import Header from './components/Header.vue'
export default {
	name: 'App',
	components: {
		Header
	},
	data() {
		return {
			myProps: {}
		}
	},
	methods: {
		addError(errMessage) {
			this.myProps = {...this.myProps, errMessage};
		},
		setSearchLocation(searchLocation) {
			this.myProps = { ...this.myProps, searchLocation };
		}
	}
}
</script>


<style>
	html, body {
		margin: 0;
		padding: 0;
		height: 100%;
	}

	#app {
		font-family: 'Avenir', Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		color: #2c3e50;
		background: lightblue;
		height: 100%;
	}

	header {
		width: 100%;
		height: 80px;
		background: black;
	}

	.weather {
		float: left;
		color: white;
		line-height: 80px;
		margin-left: 50px;
		font-size: 30px;;
	}

	#nav {
		float: right;
		width: 100px;
		margin: 0;
		padding: 0;
		margin-right: 50px;
		line-height: 80px;
		font-size: 18px;
	}

	#nav li {
		list-style: none;
		display: inline;
		margin: 5px;
		color: white;
	}

	#nav li a {
		color: white;
		text-decoration: none;
	}
</style>
