<template>
   <img class="photo" :src=" 'photos/' + photos[index]" />
</template>
<script>

const fs = require('electron').remote.require('fs')

function fetch() {
    let ar = [];
    fs.readdir("public/photos", (err, files) => {
        files.forEach(file => {
            ar.push(file);
        }); 
    });
  return ar;
} 


export default {
    data: () => {
        return {
            photos: fetch(),
            index: 0,
            slideTime: 10000,
            refreshTime: 50000
        }
    },

    methods: {
        slide: function(random = false) {

            let numberOfPhotos = this.photos.length - 1;

            if(random) {
                this.index = Math.floor(Math.random() * Math.floor(numberOfPhotos));
            }


            if(this.index < numberOfPhotos) {
                this.index = this.index + 1;
            } else {
                this.index = 0;
            }
        }
    },

    created() {
        setInterval(() => this.slide(true), this.slideTime);
        setInterval(() => this.photos = fetch(), this.refreshTime);

    },
}
</script>


<style>
.photo {
    min-height: 100%;
    min-width: 1336px;
	
    /* Set up proportionate scaling */
    width: 100%;
    height: auto;
	
    /* Set up positioning */
    position: fixed;
    top: 0;
    left: 0;
    cursor: none;
}
</style>
