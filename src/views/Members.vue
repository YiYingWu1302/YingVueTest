<template>
  <div class="members">
    <img alt="Vue logo" src="../assets/logo.png">
    <create-card description="Simple CRUD webpage with Vue.js example."/>
  </div>
</template>

<script>
// @ is an alias to /src
import CreateCard from '@/components/CreateCard.vue'

export default {
  name: 'members',
  components: {
    CreateCard
  }
}
</script>
