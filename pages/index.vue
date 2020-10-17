<template>
  <div class="container">

    <vs-card-group v-show="posts != null">
      <vs-card v-for="post in posts" @click="goTo(post.id,post.slug)">
        <template #title>
          <h3>{{ post.title.rendered }}</h3>
        </template>
        <template #img>
          <img :src="post._embedded['wp:featuredmedia']['0'].source_url" alt="">
        </template>
        <template #text>
          <p>
            {{ post.excerpt.rendered | strippedContent }}
          </p>
        </template>
        <template #interactions>
          <vs-button danger icon>
            <i class='bx bx-heart'></i>
          </vs-button>
          <vs-button class="btn-chat" shadow primary>
            <i class='bx bx-chat'></i>
            <span class="span">
            54
          </span>
          </vs-button>
        </template>
      </vs-card>
    </vs-card-group>


  </div>
</template>

<script>


    import axios from "axios";

    export default {
        data() {
            return {
                posts: null,
            }
        },
        components: {},
        methods: {
            goTo: function (pageID, slug) {
                console.log('Page id is '+ pageID);
                this.$router.push(
                    {
                        path: slug,
                    }
                );

            }
        },
        mounted() {
            const loading = this.$vs.loading({
                text: 'Loading Blog Contents ...'
            });

            axios.get('https://prappo.dev/wp-json/wp/v2/posts?_embed')
                .then((result) => {
                    console.log(result.data);
                    this.posts = result.data;
                    loading.close();
                    this.$confetti.start();
                    setTimeout(()=>{
                        this.$confetti.stop();
                    }, 5000);
                })
        },
        filters: {
            strippedContent: function (string) {
                return string.replace(/<\/?[^>]+>/ig, " ");
            }
        }
    }
</script>

<style>


  .vs-card__group .vs-card__group-cards {
    overflow: hidden !important;
  }
</style>
