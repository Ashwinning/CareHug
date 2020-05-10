<template>
	<v-app>
		<v-autocomplete
				v-model="model"
				:items="items"
				:loading="isLoading"
				:search-input.sync="search"
				chips
				clearable
				hide-details
				hide-selected
				item-text="name"
				item-value="symbol"
				label="Search for a coin..."
				solo
			>
				<template v-slot:no-data>
					<v-list-item>
						<v-list-item-title>
							Search for your favorite
							<strong>Cryptocurrency</strong>
						</v-list-item-title>
					</v-list-item>
				</template>
				<template v-slot:selection="{ attr, on, item, selected }">
					<v-chip v-bind="attr" :input-value="selected" color="blue-grey" class="white--text" v-on="on">
						<v-icon left>mdi-coin</v-icon>
						<span v-text="item.name"></span>
					</v-chip>
				</template>
				<template v-slot:item="{ item }">
					<v-col cols="4"> <v-img :src="item.thumbnailUrl" /> </v-col>
				</template>
			</v-autocomplete>
	</v-app>
</template>

<script>
	export default {
		data: () => ({
			isLoading: false,
			items: [],
			model: null,
			search: null,
			tab: null,
		}),

		watch: {
			model(val) {
				if (val != null) this.tab = 0;
				else this.tab = null;
			},
			search(val) {
				// Items have already been loaded
				if (this.items.length > 0) return;

				this.isLoading = true;

				let subscriptionKey = 'f3f76d31649040679d04ed71eb56b57e';
				let term = 'hamburger *.png';
				// Lazily load input items
				fetch('https://carehug.cognitiveservices.azure.com/bing/v7.0/images/search?q='+encodeURI(term), {
					method: 'get',
					headers : {
					'Ocp-Apim-Subscription-Key' : subscriptionKey,
					},
				}).then((data)=> data.clone().json())
				.then((res) => {
						//console.log(res.value);
						this.items = Object.values(res.value);
					})
					.catch((err) => {
						console.log(err);
					})
					.finally(() => (this.isLoading = false));



				/*
				fetch("https://api.coingecko.com/api/v3/coins/list")
					.then((res) => res.clone().json())
					.then((res) => {
						this.items = res;
					})
					.catch((err) => {
						console.log(err);
					})
					.finally(() => (this.isLoading = false)); */
			},
		},
	};
</script>
