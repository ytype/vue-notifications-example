<template>
  <div id="app" class="hero is-full has-text-centered">
    <h1 class="title">🔔 vue-notifications-example</h1>
    <b-loading :is-full-page="isFullPage" v-model="isLoading"></b-loading>
    <img class="splash" src="https://images.unsplash.com/photo-1554177255-61502b352de3?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1050&q=80">
    <p class="subtitle">click to button fetch data from jsonplaceholder and show Notification!</p>
    <button class="button is-dark" @click="loadData">🔥 loadData + 2s</button>
    <b-carousel>
        <b-carousel-item v-for="(post, i) in posts" :key="i">
            <section class="hero is-medium is-dark">
                <div class="hero-body has-text-centered">
                    <h1 class="title">{{post.title}}</h1>
                    <p>{{post.body}}</p>
                </div>
            </section>
        </b-carousel-item>
    </b-carousel>
  </div>
</template>

<script lang="ts">
import axios from 'axios'
import Vue from 'vue'
import { ToastProgrammatic as Toast } from 'buefy'

interface Post {
  userId: number;
  id: number;
  title: string;
  body: string;
}

export default Vue.extend({
  data() {
    return {
      isLoading: false,
      isFullPage: true,
      posts: [{
        userId: 2,
        id: 11,
        title: 'et ea vero quia laudantium autem',
        body: 'delectus reiciendis molestiae occaecati non minima eveniet qui voluptatibus\naccusamus in eum beatae sit\nvel qui neque voluptates ut commodi qui incidunt\nut animi commodi'
      }] as Post[]
    }
  },
  created() {
    if (!Notification) {
      Toast.open('Desktop notifications not available in your browser. Try Chromium!')
    }
    if (Notification.permission !== 'granted') {
      Notification.requestPermission()
    }
  },
  methods: {
    showNotification() {
      if (Notification.permission !== 'granted') {
        Notification.requestPermission()
      } else {
        const notification = new Notification('vue-notifications-example', {
          icon: './logo.png',
          body: 'Data load complete!'
        })
        notification.onclick = function () {
          window.open('http://stackoverflow.com/a/13328397/1269037')
        }
      }
    },
    loadData() {
      this.isLoading = true
      setTimeout(async () => {
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts')
        this.posts = response.data.slice(0, 10)
        console.log(this.posts)
        this.isLoading = false
        this.showNotification()
      }, 2000)
    }

  }
})
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+KR&display=swap');
#app {
  font-family: 'Noto Sans KR', sans-serif;
  min-height: 100vh;
}
html {
  overflow: hidden !important;
}
button {
  width: 15rem;
  align-self: center;
}
.splash {
  height: auto;
  width: 300px;
  align-self: center;
}
.title {
  align-self: center;
  margin-top: 2rem;
  margin-bottom: 2rem;
}
</style>
