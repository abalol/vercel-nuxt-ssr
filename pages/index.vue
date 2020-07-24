<template>
  <div class="container">
    <div class="logo">
      <div class="border"></div>
      <div class="border-rev"></div>
      <img class="logo_img" src="@/assets/logo.svg" />
      <h1>ブログ的な</h1>
    </div>
    <div
      v-for="(post, index) in posts"
      :key="index"
      class="card"
      @click="goDetailPage(post.key)"
    >
      <div class="title">{{ post.data.title }}</div>
      <div class="contents">{{ post.data.contents }}</div>
    </div>
  </div>
</template>

<script>
import firebase from '@/plugins/firebase'
const db = firebase.firestore()

export default {
  async asyncData(context) {
    const info = await db.collection('info').doc('VGa5gBOsAlfGKWQTSeng').get()
    const postsData = await db.collection('posts').get()
    const posts = postsData.docs.map((value) => {
      return {
        data: value.data(),
        key: value.id,
      }
    })
    return { info: info.data(), posts: posts.reverse() }
  },
  data() {},
  methods: {
    goDetailPage(key) {
      this.$router.push(`/posts/${key}`)
    },
  },
}
</script>

<style scoped>
h1 {
  display: inline-block;
}
.container {
  margin: 0 auto;
  display: flex;
  justify-content: start;
  align-items: center;
  text-align: center;
  flex-wrap: wrap;
  padding: 16px;
}
.contents {
  padding: 16px;
}
.title {
  padding: 16px 16px 0 16px;
  font-weight: bold;
}
@media screen and (max-width: 480px) {
  .logo {
    width: 100%;
  }
  .card {
    width: 100%;
    margin: 8px;
  }
}
@media screen and (min-width: 481px) {
  .logo {
    width: 240px;
  }
  .card {
    width: 240px;
    margin: 16px;
  }
}
.card {
  height: 240px;
  border-radius: 50px;
  margin-bottom: 32px;
  background: #f0f4f4;
  box-shadow: 7px 7px 12px #bbbebe, -7px -7px 12px #ffffff;
  overflow: hidden;
  cursor: pointer;
}
.post_image {
  background: #f0f4f4;
  width: 100%;
  height: 200px;
  margin: 0 auto 16px 0;
  background-image: url('~assets/sample.jpg');
}
.logo {
  height: 240px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  line-height: 32px;
  position: relative;
  margin: 16px 16px 32px 16px;
  order: -1;
  background: #f0f4f4;
  box-shadow: 7px 7px 12px #bbbebe, -7px -7px 12px #ffffff;
}
@keyframes rotation {
  0% {
    transform: rotate(0);
  }
  100% {
    transform: rotate(360deg);
  }
}
@keyframes rotation-rev {
  0% {
    transform: rotate(0);
  }
  100% {
    transform: rotate(-360deg);
  }
}
.logo_img {
  width: 80px;
  margin: 16px;
  opacity: 0.8;
}
</style>
