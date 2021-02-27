<template>
  <v-row
   class="my-9"
  >
    <v-spacer />
    <v-col
     cols="12"
     sm="6"
     md="4"
     style="text-align:center"
     @dragover.prevent
     @drop="dropOnTrash"
    >
      <v-card
       class="text-center:my-10 py-5"
       style="border:outset 5px red"
      >
        <span style="color:#CC0033; font-size:22px">
          <v-icon
           color="red"
           style="font-size:50px"
          >
            mdi-trash-can
          </v-icon>
           Drag & Dropで動画を削除
        </span>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  props: {
    droppedVideoId: {
      type: Number,
      default: 0,
    }
  },

  methods: {
    dropOnTrash() {
      console.log(this.droppedVideoId)
      this.$emit('deleteVideo', this.droppedVideoId)
    },
  }
}
</script>
