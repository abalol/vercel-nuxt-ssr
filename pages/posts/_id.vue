<template>
  <div class="container">
    <h1 class="title">
      {{ post.title }}
    </h1>
    <div class="contents">
      {{ post.contents }}
    </div>
    <div class="back">
      <n-link to="/">戻る</n-link>
    </div>
  </div>
</template>

<script>
import firebase from '@/plugins/firebase'
const db = firebase.firestore()

export default {
  async asyncData(context) {
    const id = context.params.id
    const postData = await db.collection('posts').doc(id).get()
    return { post: postData.data() }
  },
  data() {},
  methods: {},
}
</script>

<style scoped>
h1 {
  display: inline-block;
}
.container {
  border-radius: 50px;
  margin-bottom: 32px;
  background: #f0f4f4;
  box-shadow: 7px 7px 12px #bbbebe, -7px -7px 12px #ffffff;
  overflow: hidden;
  padding: 16px;
  margin: 16px;
}
.contents {
  padding: 16px;
}
.title {
  padding: 16px 16px 0 16px;
  font-weight: bold;
}
.back {
  width: 100%;
  text-align: center;
}
@media screen and (max-width: 480px) {
  .card {
    width: 100%;
    margin: 8px;
  }
}
@media screen and (min-width: 481px) {
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
}
.post_image {
  background: #f0f4f4;
  width: 100%;
  height: 200px;
  margin: 0 auto 16px 0;
  background-image: url('~assets/sample.jpg');
}
.logo {
  width: 240px;
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
