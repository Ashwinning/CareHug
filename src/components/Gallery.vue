<template>
	<v-row>
		<v-col cols="12">
			<v-card>
				<v-container fluid>
					<v-row>
						<v-col v-for="item in items" :key="item.imageId" class="d-flex child-flex" cols="4">
							<v-card flat tile class="d-flex">
								<v-img
									:src="item.thumbnailUrl"
									aspect-ratio="1"
									class="grey lighten-2"
									contain
									@click="selectImage(item)"
								>
									<template v-slot:placeholder>
										<v-row class="fill-height ma-0" align="center" justify="center">
											<v-progress-circular indeterminate color="grey lighten-5"></v-progress-circular>
										</v-row>
									</template>
								</v-img>
							</v-card>
						</v-col>
					</v-row>
				</v-container>
			</v-card>
		</v-col>
	</v-row>
</template>

<script>
	export default {
		name: "Gallery",
		props: ["items"],
		methods: {
			selectImage(item){
				this.$emit('hide-search-results', item);
			}
		}
		
	};
</script>

<style scoped>
	.contain{
		background-size: contain;
	}
</style>
