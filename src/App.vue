<template>
  <v-app>
    <v-toolbar
      max-height="60"
      dark
      src="./assets/youtube.jpg"
    >
      <v-spacer/>
      <v-tool-title
        class="header font-italic display-1"
      >
        Youtube Curation
      </v-tool-title>
      <v-spacer/>
    </v-toolbar>
    <top />
    <v-toolbar 
      dark 
    >
      <v-spacer />
      <p class="footer">@QuestAcademia</p>
    </v-toolbar>
  </v-app>
</template>

<script>
import Top from './components/Top.vue'

export default {
  name: 'App',
  components: {
    Top,
  }
}
</script>

<style>
.header {
   padding-bottom:5px;
   color:black;
   text-align:center;
 }

 .footer {
   margin-top:15px;
 }
</style>
