<template>
  <div class="center">
    <vs-card style="width: 80%; height: 100% !important;" type="1" v-show="posts != null" v-for="post in posts">
      <template #title>
        <h3>{{ post.title.rendered}}</h3>
      </template>
      <template #img>
        <img :src="post._embedded['wp:featuredmedia']['0'].source_url" alt="">
      </template>
      <template #text>
        <p v-html="post.content.rendered">

        </p>
      </template>
      <template #interactions>
        <vs-button danger icon>
          <i class='bx bx-heart'></i>
        </vs-button>
        <vs-button class="btn-chat" shadow primary>
          <i class='bx bx-chat' ></i>
          <span class="span">
          54
        </span>
        </vs-button>
        <vs-button shadow success>
          <i class='bx bxl-wordpress'></i>
          <span class="span"> See original post</span>
        </vs-button>
      </template>
    </vs-card>

  </div>
</template>

<script>
    import axios from 'axios';

    export default {
        data() {
            return {
                posts: null,
            }
        },
        methods: {},
        mounted() {

            const loading = this.$vs.loading({
                text: 'Loading Blog Contents ...'
            });
            const slug = this.slug;
            axios.get('https://prappo.dev/wp-json/wp/v2/posts?slug=' + slug + '&_embed')
                .then((result) => {
                    console.log('Data is');
                    console.log(result.data);
                    this.posts = result.data;
                    loading.close();
                    this.$confetti.start();
                    setTimeout(()=>{
                        this.$confetti.stop();
                    }, 2000);
                })


        },
        async asyncData({params}) {

            const slug = params.slug // When calling /abc the slug will be "abc"
            return {slug}
        }
    }
</script>

<style>
  /*.vs-card-content {*/
  /*  width: 100% !important;*/
  /*}*/

  .vs-card {
    max-width: 100% !important;
    max-height: 100% !important;
  }

</style>
