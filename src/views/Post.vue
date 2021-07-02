<template>
    <article class="py-12 px-4 bg-white">
        <h1 class="text-4xl text-center mb-4 font-semibold font-heading font-semibold">{{post.title.rendered}}</h1>
        <p class="text-center">
            <span>{{getPostDate(post.date)}}, by</span>
            <a class="ml-1 text-indigo-600 hover:underline" href="#">{{post.author}}</a>
        </p>
        
        <div class="max-w-3xl mx-auto">
            <p class="mb-4" v-html="post.content.rendered"></p>
        </div>
    </article>    
</template>
<script>
import axios from 'axios';
import moment from 'moment';
export default {
  name: 'Post',
  data(){
    return{
      //postsUrl:"http://localhost/php/vue-wp/wp-json/wp/v2/posts/"+this.id,
      post:[],
    }
  },
  
    created () {
    this.getPost()
    },
//   beforeRouteEnter (to, from, next) {
//     next(vm => vm.getPost())
//   },
//   // when route changes and this component is already rendered,
//   // the logic will be slightly different.
//   beforeRouteUpdate (to, from, next) {
//       this.getPost()
//       next()
//   },
  methods:{
    getPost () {
      
      axios.get(process.env.API_URL + '/wp-json/wp/v2/posts/' + id)
      .then(response => {
        this.post = response.data[0]

        if (this.post === undefined) {
          this.$router.push({name: 'NotFound'})
        }
        document.title = this.post.title.rendered
      })
      .catch(e => {
        console.log(e)
      })
    },
    getPostDate(date){
      return moment(date).format("lll");
    },
    getYear(date){
      return moment(date).format("Y");
    },
    getMonth(date){
      return moment(date).format("M");
    },
    getDay(date){
      return moment(date).format("D");
    }


  },
  mounted(){
    this.getRecentMessages();
  },
  props: {
    msg: String
  }
}
</script>
