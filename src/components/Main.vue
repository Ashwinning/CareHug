<template>
	 <v-container>
		<v-row xs="12" dense>
			<v-col cols="12">
				<Search/>
			</v-col>
			<!-- <v-col cols="2">
				<v-card
					><v-card-text><v-icon>mdi-upload</v-icon></v-card-text></v-card
				>
			</v-col> -->
		</v-row>
		<v-row xs="12">
			<v-col cols="12">
				<v-card ref="canvasCard">
					<canvas ref="canvas"></canvas>
				</v-card>
			</v-col>
		</v-row>
		<v-row>
			<v-col>
				<v-btn large @click="saveImg"> <v-icon>mdi-content-save</v-icon> Save </v-btn>
			</v-col>
		</v-row>
	</v-container> 
</template>

<script>
	import Search from "@/components/Search"; 
	import hugBase from "@/assets/1ef.png";
	import hugArms from "@/assets/036.png";
	import tempImg from "@/assets/tmp.png";
	import transparent from "@/assets/transparent.png";
	export default {
		name: "Main",
		components:{
			Search
		},
		data() {
			return {
				canvas: null,
				meme: {
					images: [transparent, hugBase, tempImg, hugArms],
					canvasImages: [], //These get drawn on the canvas
					imgCount: 0
				},
			};
		},
		methods: {
			DrawImagesToCanvas(image) {
				var canvas = this.canvas;
				canvas.clearRect(0, 0, canvas.canvas.width, canvas.canvas.height);
				//reset
				this.meme.canvasImages = [];
				this.imgCount = 0;
				//add new image
				this.meme.images[2] = image;
				//store refs for anonymous functions
				var imgCount = this.meme.imgCount;
				var canvasImages = this.meme.canvasImages;
				for (var i = 0; i < this.meme.images.length; i++) {
					var img = new Image();
					this.meme.canvasImages.push(img);
					img.onload = function() {
					imgCount++;
						if (imgCount >= canvasImages.length) {
							// imgs[] now contains all your images in imgURLs[] order
							for (var x in canvasImages){
								var l,t,w,h;
								if (x != 2){
									l = 0;
									t = 0;
									w = canvas.canvas.width;
									h = canvas.canvas.height;
								}
								else{
									l = canvas.canvas.width/4.7;
									t = canvas.canvas.width/2.2;
									w = canvas.canvas.width/1.9;
									h = canvas.canvas.height/1.9; 
								}
								canvas.drawImage(canvasImages[x], l, t, w, h);
							}
						}
					};
					img.src = this.meme.images[i];
				}
			},
			saveImg(){
				
			}
		},
		mounted() {
			const canvas = this.$refs.canvas.getContext("2d");
			this.canvas = canvas;
			var width = this.$refs.canvasCard.$el.offsetWidth;
			console.log(width);
			canvas.canvas.width = width;
			canvas.canvas.height = width;
			this.DrawImagesToCanvas(tempImg);
			this.$root.$on('set-image', (item) =>{
				//console.log('setting image', item.contentUrl);
				this.DrawImagesToCanvas(item.contentUrl);
			})
		},
		
	};
</script>

<style>
	canvas {
		background-color: transparent;
	}
</style>
