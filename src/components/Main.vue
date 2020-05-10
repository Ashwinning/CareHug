<template>
	<v-container>
		<v-row xs="12" dense>
			<v-col cols="10">
				<v-card><v-card-text>Search</v-card-text></v-card>
			</v-col>
			<v-col cols="2">
				<v-card
					><v-card-text><v-icon>mdi-upload</v-icon></v-card-text></v-card
				>
			</v-col>
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
				<v-btn large> <v-icon>mdi-content-save</v-icon> Save </v-btn>
			</v-col>
		</v-row>
	</v-container>
</template>

<script>
	import hugBase from "@/assets/1ef.png";
	import hugArms from "@/assets/036.png";
	import tempImg from "@/assets/tmp.png";
	export default {
		name: "Main",
		data() {
			return {
				canvas: null,
				meme: {
					images: [hugBase, tempImg, hugArms],
					canvasImages: [], //These get drawn on the canvas
					imgCount: 0
				},
			};
		},
		methods: {
			DrawImagesToCanvas() {
				//reset
				this.meme.canvasImages = [];
				this.imgCount = 0;
				//store refs for anonymous functions
				var imgCount = this.meme.imgCount;
				var canvasImages = this.meme.canvasImages;
				var canvas = this.canvas;
				for (var i = 0; i < this.meme.images.length; i++) {
					var img = new Image();
					this.meme.canvasImages.push(img);
					img.onload = function() {
					imgCount++;
						if (imgCount >= canvasImages.length) {
							// imgs[] now contains all your images in imgURLs[] order
							for (var x in canvasImages){
								var l,t,w,h;
								if (x != 1){
									l = 0;
									t = 0;
									w = canvas.canvas.width;
									h = canvas.canvas.height;
								}
								else{
									l = canvas.canvas.width/4.5;
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
		},
		mounted() {
			const canvas = this.$refs.canvas.getContext("2d");
			this.canvas = canvas;
			var width = this.$refs.canvasCard.$el.offsetWidth;
			console.log(width);
			canvas.canvas.width = width;
			canvas.canvas.height = width;
			this.DrawImagesToCanvas();
		},
	};
</script>

<style>
	canvas {
		background-color: bisque;
	}
</style>
