<template lang="pug">
#Edit
  section
    .container
      router-link.fas.fa-arrow-left( :to="{name: 'Post', params: {slug: $route.params.slug}}")
      form
        .form-group
          label(for='exampleInputEmail1') Edit the page
          p #[strong Author:] "{{ss.currentAuthor[0].username}}"  {{ss.currentAuthor[0].name}}
          input.form-control.title(  v-model="title.value") 
          textarea(  v-model="body.value" @keypress.enter.prevent="updatePost(title, body, $route.params.slug)")
        button.btn.btn-primary(type='submit' @click.prevent="updatePost(title, body, $route.params.slug)") Update
    
</template>

//
//
//
//
//
//
//
//
//

<script lang="ts">
// Possible reusable functions placed in other files
import { useStore } from "vuex";
import { onMounted, ref, provide, onBeforeMount, onUpdated } from "vue";
import { useRouter, useRoute } from "vue-router";
import useData from "@/composables/useData";
// import {fire, fireAuth} from '@/firebase/config'
// import { a, n } from "@/composables/types";
// import useCss from "@/composables/useCss";
export default ({

  setup(){
    const ss = useStore().state;
    const route = useRoute();
    const axios = require('axios').default;
    const {getPosts, getAuthors, getComments, sendComment, updatePost} = useData();
    ///////////////////////

    // // v-model variables
    const title = ref('')
    const body = ref('')

    // // if the current post has beed defined then change title and body values
    const checkUpdate = () =>{
      if(ss.currentPost){
        title.value = ref(ss.currentPost[0].title)
        body.value = ref(ss.currentPost[0].body)
      }
    }

    // // after we have the title and body then PUT to url
    // const updatePost = () =>{
    //   axios.put('https://jsonplaceholder.typicode.com/posts/' + route.params.slug, {
    //     title: title.value,
    //     body: body.value
    //   }).then(response =>{
    //     alert(`title: \n ${response.data.title._rawValue} \n body: \n ${response.data.body._rawValue}`)
    //   }).catch(err => {
    //     console.log(err)
    //   })
    // }



    // ss.currentPost = ss.response.data.filter(item => {
    //   return item.id == route.params.slug; 
    // })
    getPosts( (response)=>{
      ss.posts = response

      // // save current post data
      ss.currentPost = response.data.filter(item => {
        return item.id == route.params.slug;  
      })
      getAuthors((response)=>{
        ss.authors = response

        // // save current author data using current post data
        ss.currentAuthor = response.data.filter(item => {
          return item.id == ss.currentPost[0].userId;
        })
      })

    })
    
    
    
    onUpdated(()=>{
      checkUpdate()
    })




    // // js for css ... 

    // // js after the page has been mounted 
    // onMounted(() => {
    // });
    
    return {
      ss, updatePost, title, body, route
    };
  }

});
</script>

//
//
//
//
//
//
//
//

<style lang="scss">
// uni scss is imported globaly, but for use of variables uniTheme.scss is imported every time 
// @import 'x../assets/uniTheme.scss'
#Edit{
   section{}
}

</style>
