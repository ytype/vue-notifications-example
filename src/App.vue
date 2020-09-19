<template>
  <div id="app">
    <b-loading :is-full-page="isFullPage" v-model="isLoading"></b-loading>
    <b-carousel>
        <b-carousel-item v-for="(post, i) in posts" :key="i">
            <section class="hero is-medium">
                <div class="hero-body has-text-centered">
                    <h1 class="title">{{post.title}}</h1>
                    <p>{{post.body}}</p>
                </div>
            </section>
        </b-carousel-item>
    </b-carousel>
    <button class="button" @click="loadData">loadData</button>
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
      posts: [] as Post[]
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
        this.posts = response.data
        console.log(this.posts)
        this.isLoading = false
        this.showNotification()
      }, 1000)
    }

  }
})
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+KR&display=swap');
#app {
  font-family: 'Noto Sans KR', sans-serif;
}
</style>
