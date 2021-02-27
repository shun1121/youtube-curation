<template>
  <v-card
    class="my-10"
    :elevation="formElevation"
    :rounded="formRounded"
    width="100%"
  >
    <v-container
      class="px-10 text-center"
    >
      <v-form
        ref="form"
        class="mx-16 mt-5 mb-3"
      >
        <v-text-field
          label="Youtube動画ID"
          v-model="videoUrl"
          :error-messages="videoErrorMessages"
          placeholder="動画URLの【v= 】の後に続く英数字"
          style="width: 300px"
        />

        <v-text-field
          label="コメント"
          v-model="comment"
          :rules="commentRules"
          placeholder="動画紹介のための任意のコメント"
          class="mb-3"
          style="width: 500px"
        />
        <v-btn
          @click="clickPost"
          type="button"
          large
        >
          投稿
        </v-btn>
      </v-form>
    </v-container>
  </v-card>
</template>

<script>
  export default {
      name: 'InputForm',

      props: {
        formRounded: {
          type: String,
          required: false,
          default: 'sm'
        },
        formElevation: {
          type: String,
          required: false,
          default: '1',
        },
      },

      data () {
          return {
              videoUrl: "",
              videoErrorMessages: [],
              comment: "",
              commentRules: [
                value => !!value || '入力してください',
                value => value.length <= 16 || '16文字以内で入力してください'
              ],
          }
      },
      watch: {//データが変更された瞬間処理を行う
        videoUrl: function (value) {
          this.validateVideoUrl(value)
        }
      },
      methods: {
        validateVideoUrl (value) {
          const stringValidation = /^[a-zA-Z0-9!-/:-@¥[-`{-~]+$/.test(value)
          console.log(stringValidation) //正規表現に当てはまっていたらtrue、違ったらfalseを返す。
          if (!value) {
            this.videoErrorMessages = ['入力してください',]
          } else if (value.length !== 11) {
            this.videoErrorMessages = ['11文字で入力してください',]
          } else if (!stringValidation) {
            this.videoErrorMessages = ['半角英数記号で入力してください',]
          } else {
            this.videoErrorMessages = []
          }
        },
        clickPost () {
          this.validateVideoUrl(this.videoUrl)//validateVideoUrlメソッドの呼び出し、バリデーションを実行
          const validateComment = this.$refs.form.validate()//すべての入力を検証し、すべて有効であるかどうか確認
          if (this.videoErrorMessages.length === 0 && validateComment) {
            this.$emit('storeVideo', this.videoUrl, this.comment)
          }
        }
      }
  }
</script>


