<template>
  <v-container>
    <input-form
      formRounded="xl"
      :formElevation="formElevation"
      @storeVideo="storeVideo"
    />
    
    <v-messages
      :value="responseError"
      color="red"
      class="response-error my-5 text-center"
    />

    <videos
      ref="movies" 
      :video-items="videoItems"
      :loading="loading"
      @deleteVideo="deleteVideo"
    />
  </v-container>
</template>

<script>
import InputForm from './InputForm.vue'
import Videos from './Videos.vue'
import axios from 'axios'

export default {
    name: 'Top',
    components: {
      InputForm,
      Videos,
    },
    data () {
      return {
        formElevation: "10",
        videoItems: [{}],
        responseError: [],
        loading: true,
      }
    },
    created () {//ページをロードしたタイミングで実行される
      this.getVideos()
    },

    methods: {
      getVideos () {
        axios.get('https://youtube-curation.herokuapp.com/rest/1')
        .then((response) => {//responseはサーバから来たデータ（上のURLのオブジェクト）
          this.videoItems = response.data.user.movies
        }).catch((error) => {//失敗時
          console.log(error)
          this.responseError = ['動画の取得に失敗しました']
        }).finally(() => {//通信成功でも失敗でもとおる処理
          setTimeout(() => {
            this.loading = false//1秒後にアイコン非表示
          }, 1000)
          this.$refs.movies.init()//refを取得,Videosコンポーネントの初期化を実施
        }) 
      },

      storeVideo (videoUrl, comment) {
        console.log(videoUrl, comment)
        this.loading = true
        this.responseError = []
        // ↓ 第二引数には第一引数のURLに送りたいパラメータを指定
        axios.post('https://youtube-curation.herokuapp.com/rest', {
          url: videoUrl,
          comment: comment,
        }).then((response) => {
          //console.log(response)//{data: {…}, status: 200, statusT.....}
          //console.log(response.data)//{movies: Array(8)}
          //console.log(response.data.movies)//comment:(...) created_at:(...) id:(...)updated_at:(...) url:(...) user_id:(...)
          this.videoItems = response.data.movies
        }).catch((error) => {
          console.log(error)
          this.responseError = ['動画の投稿に失敗しました']
        }).finally(() => {
          setTimeout(() => {
            this.loading = false
          }, 1000)
          this.$refs.movies.init() 
        })
      },

      deleteVideo(id) {
        this.loading = true
        this.responseError = []
        axios.delete('https://youtube-curation.herokuapp.com/rest/' + id
        ).then((response) => {
          console.log(response.data.movies)
          console.log("----------------------")
          this.videoItems = response.data.movies
          setTimeout(() => {
            this.loading = false
          }, 1000)
        }).catch((error) => {
          console.log(error)
          this.responseError = ['動画の削除に失敗しました']
        }).finally(() => {
          this.$refs.movies.init()
        })
      }
    }
}
</script>
<style>
  /* 下の.v-messages__messageは、vuetifyのv-messagesコンポーネントに割り振られているクラス名 */
  .response-error .v-messages__message {
    font-size: 18px
  }
</style>