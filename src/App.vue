<script setup>
import "./css/midjourney.css"
import axios from 'axios'
import $, { event } from 'jquery'

</script>

<template>

  <body class="flex h-full flex-col text-white text-base">
    <div id="__next">

      <section id="playground" aria-label="playground" class="relative overflow-hidden pt-40">
        <div class="AnimateDiff_gradient_block"></div>
        <div class="relative mx-auto px-4 lg:px-0 max-w-[672px] lg:max-w-[944px]">
          <div class="w-full mb-6 lg:mb-12">
            <h2
              class="AnimateDiff_gradient_text_color_animate mx-auto mb-6 w-fit text-center font-acronym text-4xl font-bold md:text-5xl lg:text-6xl !leading-tight">
              Midjourney</h2>
            <p class="mx-auto px-4 text-center font-normal text-neutral-40 md:text-lg lg:text-xl">
              Using Midjourney
              without
              discord,Creating stunning AI art with easy prompt</p>
          </div>
          <div>
            <div class="relative top-0 mx-auto w-full max-w-screen-lg items-center lg:flex">
              <label class="AnimateDiff_input_container w-full p-1.5 rounded-md flex items-center justify-between"
                for="prompt">
                <input data-styles-target="prompt" name="prompt" autocomplete="off"
                  class="w-[85%] px-2 text-sm font-medium border-none bg-transparent placeholder-gray-400 outline-none focus:ring-0"
                  placeholder="Describe anything in your mind, use short sentences, separated by commas"
                  v-model="prompt">
                <div class="inline-block">
                  <button
                    class="w-[120px] rounded-full px-2 py-2.5 text-base text-center text-white font-bold text-ellipsis overflow-hidden whitespace-nowrap bg-gradient-to-r from-[#FCAC23] to-[#14B8A6] hover:bg-gradient-to-l inline-flex items-center justify-center"
                    type="submit" v-on:click="sendPrompt()">
                    Generate
                  </button>
                </div>
              </label>
            </div>
          </div>
          <div class="w-full mt-6 flex items-center justify-center"></div>
        </div>
      </section>
    </div>


        <div class="image-container">
          <div id="image" class="image" v-for="(image, index) in imageList" :key="index">
              <img :src="image" v-on:click="toFullScreen(image)"/>
          </div>
        </div>

        <!-- The Modal -->
        <div id="myModal" class="modal">
          <span class="close"  v-on:click="closeFullScreen()">&times;</span>
          <img class="modal-content" id="img01">
          <div id="caption"></div>
        </div>
  </body>

</template>

<script>

export default {
  
  name: "App",

  //components: { VueImg },

  data() {
    return {
      apiUrl: "https://api.godev.live/diffusion/",
      prompt: "",
      imageList: [],
    }
  },

  mounted() {
      this.getImageList()
  },

  methods: {

    sendPrompt() {
      //console.log(this.prompt)

      if (this.prompt.trim() == ""){
        alert("Don't leave blank")
        return;
      }

      const data = {
        prompt: this.prompt,
      };

      axios.post(this.apiUrl + 'send_prompt', data, {

        headers: {
          'apikey': 'A4C5235CB7382F2B',
          'Content-Type': 'application/json'
        }
      })
        .then(response => {
          console.log(response.data)
        })
        .catch(error => {
          console.log(error)
        })

        alert('Please wait about 30 seconds then fresh browser for your image')
        this.prompt=""
    },

    getImageList() {

      axios.get(this.apiUrl + 'collections', {

        headers: {
          'apikey': 'A4C5235CB7382F2B',
          'count': '100',
          'page': '1'
        }
      })
        .then(response => {
          console.log(response.data)
          const imageDataList = response.data.image_list
          imageDataList.forEach((element) => this.imageList.push(element.image_url));

        })
        .catch(error => {
          console.log(error)
        })
    },

    toFullScreen(image){
      console.log("full")
      $('#myModal').css("display", "block");
      $("#img01").attr("src",image);

    },

    closeFullScreen(){
      $('#myModal').css("display", "none");
    }

  }
}
</script>

<style>
      /* #app {
        width: 100%;
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
      }
      h2{
        text-align: center;
      } */
      .image-container{
        width: 80%;
        margin-left: 10%;
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        gap: 5px;
      }
     

      #image img{
        width: 100%;
        height: 100%;
      }

      #image {
        -webkit-transform: scale(1);
	      transform: scale(1);
	      -webkit-transition: .3s ease-in-out;
	      transition: .3s ease-in-out;
        opacity: 1;
        
      }

      #image:hover {
        -webkit-transform: scale(1.05);
	      transform: scale(1.05);
        opacity: 0.5;
      }

      /* The Modal (background) */
.modal {
  display: none; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  padding-top: 100px; /* Location of the box */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0,0,0); /* Fallback color */
  background-color: rgba(0,0,0,0.9); /* Black w/ opacity */
}

/* Modal Content (image) */
.modal-content {
  margin: auto;
  display: block;
  width: 80%;
  max-width: 500px;

}

/* Caption of Modal Image */
#caption {
  margin: auto;
  display: block;
  width: 80%;
  max-width: 700px;
  text-align: center;
  color: #ccc;
  padding: 10px 0;
  height: 150px;
}

/* Add Animation */
.modal-content, #caption {  
  -webkit-animation-name: zoom;
  -webkit-animation-duration: 0.3s;
  animation-name: zoom;
  animation-duration: 0.3s;
}

@-webkit-keyframes zoom {
  from {-webkit-transform:scale(0)} 
  to {-webkit-transform:scale(1)}
}

@keyframes zoom {
  from {transform:scale(0)} 
  to {transform:scale(1)}
}

/* The Close Button */
.close {
  position: absolute;
  top: 15px;
  right: 35px;
  color: #f1f1f1;
  font-size: 40px;
  font-weight: bold;
  transition: 0.3s;
}

.close:hover,
.close:focus {
  color: #bbb;
  text-decoration: none;
  cursor: pointer;
}

/* 100% Image Width on Smaller Screens */
@media only screen and (max-width: 700px){
  .modal-content {
    width: 100%;
  }
}
    
</style>

