<template>
  <div class="container">
    <div v-if="!isAdmin">
      Password<input v-model="pass" type="password" /><br />
      <button @click="checkPassword()">Login</button><br />
      Generate Secret Hash<input v-model="hashBaseText" type="text" />
      <button @click="generateHash()">Generate</button><br />
      {{ secretHash }}
    </div>
    <div v-else>
      <button v-if="editKey" @click="createPost()">新規記事作成</button>
      <button v-if="editKey" class="delete-button" @click="deletePost()">
        削除</button
      ><br />
      <div>Post ID : {{ editKey }}</div>
      <p class="label">記事タイトル</p>
      <input v-model="title" class="input" type="text" />
      <p class="label">本文</p>
      <textarea v-model="contents" class="textarea" type="text" />
      <h1 class="title-preview" v-html="title"></h1>
      <div class="preview" v-html="contents"></div>
      <button v-if="!editKey" @click="post()">投稿</button><br />
      <button v-if="editKey" @click="updatePost()">更新</button><br />
      <hr />
      <h3>Editing a posted item.</h3>
      <li
        v-for="(p, index) in posts"
        :key="index"
        class="titles"
        @click="edit(p.key)"
      >
        {{ p.data.title }}
      </li>
    </div>
  </div>
</template>

<script>
import JSSHA from 'jssha'
import firebase from '@/plugins/firebase'
const db = firebase.firestore()

export default {
  async asyncData(context) {
    if (context.query.pass && context.query.pass !== '') {
      const secret = await db.collection('pass').doc('secret').get()
      if (secret.data().hash === context.query.pass) {
        const postsData = await db.collection('posts').get()
        const posts = postsData.docs.map((value) => {
          return {
            data: value.data(),
            key: value.id,
          }
        })
        return { isAdmin: true, posts: posts.reverse() }
      } else {
        return { isAdmin: false }
      }
    } else {
      return { isAdmin: false }
    }
  },
  data() {
    return {
      title: '',
      contents: '',
      hashBaseText: '',
      secretHash: '',
      pass: '',
      editKey: null,
    }
  },
  methods: {
    createPost() {
      this.title = ''
      this.contents = ''
      this.editKey = null
    },
    async edit(key) {
      this.editKey = key
      const postData = await db.collection('posts').doc(key).get()
      const tmp = postData.data()
      this.title = tmp.title
      this.contents = tmp.contents
    },
    async post() {
      const tmp = await db.collection('posts').add({
        title: this.title,
        contents: this.contents,
      })
      this.$router.push(`/posts/${tmp.id}`)
    },
    async updatePost() {
      await db.collection('posts').doc(this.editKey).set({
        title: this.title,
        contents: this.contents,
      })
      this.$router.push(`/posts/${this.editKey}`)
    },
    async deletePost() {
      await db.collection('posts').doc(this.editKey).delete()
      location.reload()
    },
    checkPassword() {
      const shaObj = new JSSHA('SHA-512', 'TEXT')
      shaObj.update(`${this.pass}`)
      const hash = shaObj.getHash('HEX')
      window.location.href = `/admin?pass=${hash}`
    },
    generateHash(key) {
      const shaObj = new JSSHA('SHA-512', 'TEXT')
      shaObj.update(`${this.hashBaseText}`)
      const hash = shaObj.getHash('HEX')
      this.secretHash = hash
    },
  },
}
</script>

<style>
@font-face {
  font-family: 'YuGothic M';
  src: local('Yu Gothic Medium'), local('Yu Gothic');
  font-weight: 500;
}
.delete-button {
  color: #f00;
  background-color: pink;
  border: 1px solid #f00;
}
.titles {
  background-color: #ffffff;
  cursor: pointer;
  text-decoration: underline;
}
.container {
  padding: 16px;
}
.dummy {
  background-color: #d5d8dd;
  background-color: #5ca2be;
  background-color: #135487;
  background-color: #2a4353;
  background-color: #989da4;
}
.preview {
  font-family: '游ゴシック体', YuGothic, 'YuGothic M', sans-serif;
}
.label {
  width: 100%;
  color: #d5d8dd;
  background-color: #135487;
  padding: 8px;
  border-radius: 4px 4px 0 0;
}
</style>
