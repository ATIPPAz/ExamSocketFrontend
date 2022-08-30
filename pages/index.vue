<template>
  <ChatApp/>
</template>

<script lang="ts">
import Vue from 'vue'
import ChatApp from '@/components/ChatApp.vue'
export default Vue.extend({
  name: 'IndexPage',
  components:{
    ChatApp
  }
})
</script>
