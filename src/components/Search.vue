<template>
	<div>
		<v-text-field label="Solo" solo v-model="search"></v-text-field>
		<Gallery v-if="showResults" @hide-search-results="hideSearchResults" :items="items" />
	</div>
</template>

<script>
	import Gallery from "../components/Gallery";
	import _ from 'lodash';

	export default {
		name: "Search",
		components: {
			Gallery,
		},
		data: () => ({
			isLoading: false,
			items: [],
			model: null,
			search: null,
			tab: null,
			search: '',
			showResults: false,
		}),
		methods:{
			hideSearchResults(item){
				console.log('hiding results');
				this.showResults = false;
				this.$root.$emit('set-image', item);
			}
		},
		watch: {
			search: _.debounce(function(val){
				if (val.length > 0)
				{
					this.showResults = true;
				}
				else {
					this.showResults = false;
				}

				this.isLoading = true;

				let subscriptionKey = "f3f76d31649040679d04ed71eb56b57e";
				let term = val + " *.png";
				// Lazily load input items
				fetch("https://carehug.cognitiveservices.azure.com/bing/v7.0/images/search?q=" + encodeURI(term), {
					method: "get",
					headers: {
						"Ocp-Apim-Subscription-Key": subscriptionKey,
					},
				})
					.then((data) => data.clone().json())
					.then((res) => {
						//console.log(res.value);
						this.items = Object.values(res.value);
					})
					.catch((err) => {
						console.log(err);
					})
					.finally(() => (this.isLoading = false));
			}, 500)
		},
	};
</script>
