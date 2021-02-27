<template>
  <div>
    <v-row>
      <template
        v-if="loading"
      >
        <v-container
          class="px-10 py-10"
          style="text-align:center"
        >
          <v-progress-circular
            indeterminate
            size="50"
          />
        </v-container>
      </template>
      <template
        v-else
      >
        <v-col  
          v-for="item in videoInternalItems" :key="item.id"
          cols="12"
          sm="6"
          md="4"
          class="pr-0"
          style="width:290px"
        > 
          <div style="text-align:center">
            <iframe
              :src="item.url"
              width="290"
              height="163.125"
              frameborder="0"
            />
            <v-card
              draggable
              @dragstart="dragVideo(item.id)"
              color="#e5e5e5"
              height="50"
              width="290"
              style="margin:auto"
              outlined
            >
              <v-col>
                <p>{{ item.comment }}</p>
              </v-col>
            </v-card>
          </div>
        </v-col>
      </template>
    </v-row>
    <trash
      :dropped-video-id="droppedVideoId"
      @deleteVideo="deleteVideo"
    />
  </div>
</template>

<script>
import Trash from './Trash.vue'

export default {
  props: {
    videoItems: {//オブジェクト型の配列
      type: Array,
      default: null,
    },
    loading: {
      type: Boolean,
      required: true,
      default: true,
    }
  },

  components: {
    Trash,
  },

  data () {
    return {
      videoInternalItems: [],
      droppedVideoId: 0,
    }
  },

  methods: {
    init() {//$emitしなくても親で使えるの？
      this.videoInternalItems = []
      Object.keys(this.videoItems).forEach(item => {
        console.log("++++++++++++++++++")
        console.log(item)// <- videoItemsのindex番号
        // ↑ console.log(response.data.movies) -> comment:(...) created_at:(...) id:(...) updated_at:(...) url:(...) user_id:(...)と同じ
        const newItem = {
          id: this.videoItems[item].id,
          url: 'https://www.youtube.com/embed/' + this.videoItems[item].url + '?controls=1&loop=1&playlist=' + this.videoItems[item].url,
          comment: this.videoItems[item].comment,
        }
        this.videoInternalItems.push(newItem)
        // console.log(videoInternalItems)
      })
      this.videoInternalItems.sort(this.descending)
      console.log("----------------------")
      console.log(this.videoInternalItems)
    },

    descending(a, b) {
      let comparison = 0
      if (a.id > b.id) {
        comparison = -1
      } else if (b.id > a.id) {//bがaより大きい（新しい）とき前に並び替えする
        comparison = 1
      }
      return comparison
    },

    dragVideo(id) {
      console.log(id)
      this.droppedVideoId = id
    },

    deleteVideo (id) {
      this.$emit('deleteVideo', id)
    }
  }
}
</script>




