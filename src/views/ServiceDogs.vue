<template>
    
<div class="programs">
   <div class="mb-4 full-back-img"  :style="{backgroundImage:`url(${this.serviceDogImgUrl})`}"> </div>

   <!-- show loading spinner while retrieved page content from Wordpress -->
   <div v-if="contentLoading" class="text-center"><b-spinner label="Spinning" ></b-spinner></div>

   <div v-else>
      <b-container class="page-content"> 
         
         <!--What is a Service Dog section-->
         <b-card>
            <b-card-title title-tag="h1" class="text-center mb-5">{{this.serviceDogTitle}}</b-card-title>
            <b-card-text class="mt-5">
               <span v-html="this.serviceDogDescription"></span>
            </b-card-text>
         </b-card>
      
   <!-- Service Dog for Vets section-->
         <b-card id="vets" no-body  >
            <b-row>
               <b-col md="4" >
               <b-card-img :src="this.vetImgUrl" alt="this.vetImgAltTag" class="rounded-0"></b-card-img>
               </b-col>
               <b-col md="8">
               <b-card-body title-tag="h2" :title="this.vetTitle">
                  <b-card-text >
                     <span v-html="this.vetDescription"></span>

                  </b-card-text>
               </b-card-body>
               </b-col>
            </b-row>
         </b-card>

      <!-- Service Dog for Autism section-->
         <b-card no-body id="autism" >
            <b-row>
               <b-col md="4">
               <b-card-img :src="this.autismImgUrl" alt="this.autismImgAltTag"  class="rounded-0"></b-card-img>
               </b-col>
               <b-col md="8">
               <b-card-body title-tag="h2" :title="this.autismTitle">
                  <b-card-text >
                     <span v-html="this.autismDescription"></span>

                  </b-card-text>
               </b-card-body>
               </b-col>
            </b-row>
         </b-card>
   
   
         <!--how to apply for a service dog -->
         <b-card id="apply-for-dog" no-body  >
            <b-row>
               <b-col md="4">
               <b-card-img :src="this.applyForDogImgUrl" alt="this.applyForDogImgAltTag"  class="rounded-0"></b-card-img>
               </b-col>
               <b-col md="8" >
               <b-card-body title-tag="h2" :title="this.applyForDogTitle">
                  <b-card-text >
                     <span v-html="this.applyForDogDescription"></span>
                     <b-button variant="success"><router-link to="applications">Apply here</router-link></b-button>
                  </b-card-text>
               </b-card-body>
               </b-col>
            </b-row>
         </b-card>
      </b-container>
      </div>


</div>
</template>

<script>

import axios from 'axios';
import Wordpress from '@/common/wordpress.js';
import CommonMethods from '@/common/csdFunctions.js'

export default {
  name: "ServiceDogs",


  data () {
    return {
      //For API calls
      results: null,
      wordpressServiceDogsUrl:  Wordpress.wordpressUrl + Wordpress.wordpressCategoryFilter + Wordpress.serviceDogsId,
      contentLoading: true,
      // For Wordpress data
      posts: [],
      serviceDogSlug:"what-is-a-service-dog",
      serviceDogPost: [],
      serviceDogTitle: "",
      serviceDogDescription: "",
      serviceDogImgUrl: "",

      vetSlug: "veterans",
      vetPost: [],
      vetTitle: "",
      vetDescription: "",
      vetImgUrl: "",
      vetImgAltTag: "",

      autismSlug: "autism",
      autismPost: [],
      autismTitle: "",
      autismDescription: "",
      autismImgUrl: "",
      autismImgAltTag: "",

      applyForDogSlug: "apply-for-dog",
      applyForDogPost: [],
      applyForDogTitle: "",
      applyForDogDescription: "",
      applyForDogImgUrl: "",
      applyForDogImgAltTag: ""
    };
  },

  created: function() { 
     this.contentLoading = true;
    axios
    .get(this.wordpressServiceDogsUrl, {})
    .then(response => {
      this.results = response.data;
      for (let post in this.results) {
        this.posts.push(this.results[post]);
      }

      this.serviceDogPost = CommonMethods.getPostBySlug(this.results, this.serviceDogSlug);
      this.serviceDogTitle = this.serviceDogPost.title.rendered;
      this.serviceDogDescription = this.serviceDogPost.content.rendered;
      this.serviceDogImgUrl = this.serviceDogPost.acf.image1
     

      this.vetPost = CommonMethods.getPostBySlug(this.results, this.vetSlug);
      this.vetTitle = this.vetPost.title.rendered;
      this.vetDescription = this.vetPost.content.rendered;
      this.vetImgUrl = this.vetPost.acf.image1;
      this.vetImgAltTag = this.vetPost.acf.image1_alt_text;

      this.autismPost = CommonMethods.getPostBySlug(this.results, this.autismSlug);
      this.autismTitle = this.autismPost.title.rendered;
      this.autismDescription = this.autismPost.content.rendered;
      this.autismImgUrl = this.autismPost.acf.image1;
      this.autismImgAltTag = this.autismPost.acf.image1_alt_text;

      this.applyForDogPost = CommonMethods.getPostBySlug(this.results, this.applyForDogSlug);
      this.applyForDogTitle = this.applyForDogPost.title.rendered;
      this.applyForDogDescription = this.applyForDogPost.content.rendered;
      this.applyForDogImgUrl = this.applyForDogPost.acf.image1;
      this.applyForDogImgAltTag = this.applyForDogPost.acf.image1_alt_text;

      this.contentLoading = false;
      })
     
    .catch(error => {
      console.log("error accessing WordPress data" + error);
      });
  },

  
}
</script>
