<template>
  <div>    
    <div class="mt-6" v-for="(post,index) in posts" v-bind:key="index">
        <div class="max-w-4xl px-10 py-6 mx-auto bg-white rounded-lg shadow-md">
            <div class="flex items-center justify-between"><span class="font-light text-gray-600">{{getPostDate(post.date)}}</span><span v-for="(cat,index) in post.categories" v-bind:key="index"><a href="#"
                    class="px-2 py-1 font-bold text-gray-100 bg-gray-600 rounded hover:bg-gray-500">{{cat}}</a></span>
            </div>
            <div class="mt-2"><a href="#" class="text-2xl font-bold text-gray-700 hover:underline">{{post.title.rendered}}</a>
                <p class="mt-2 text-gray-600" v-html="post.excerpt.rendered"></p>
            </div>
            <div class="flex items-center justify-between mt-4"><router-link :to="'/blog/'+getYear(post.date)+'/'+getMonth(post.date)+'/'+getDay(post.date)+'/'+post.slug" class="text-blue-500 hover:underline">Read more</router-link>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import moment from 'moment';
export default {
  name: 'HelloWorld',
  data(){
    return{
      postsUrl:"http://localhost/php/vue-wp/wp-json/wp/v2/posts",
      queryOptions:{
        per_page:10,
        page:1,
        _embed:true
      },
      posts:[]
    }
  },
  beforeRouteEnter (to, from, next) {
    next(vm => vm.getRecentMessages())
  },
  // when route changes and this component is already rendered,
  // the logic will be slightly different.
  beforeRouteUpdate (to, from, next) {
      this.getRecentMessages()
      next()
  },
  methods:{
    getRecentMessages(){
      axios.get(this.postsUrl, {params:this.queryOptions})
      .then(response=>{
        this.posts=response.data;
        console.log("Posts retrieved!");
        console.log(this.posts);
      })
      .catch(error=>{console.log(error)});
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