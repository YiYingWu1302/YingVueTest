<template>
  <div class="create-person">
    <img alt="Vue logo" src="../assets/logo.png">
    <CreateCard description="Simple CRUD webpage with Vue.js example."/>
  </div>
</template>

<script>
// @ is an alias to /src
import CreateCard from '@/components/CreateCard.vue'

export default {
  name: 'create-card',
  components: {
    CreateCard
  }
}
</script>
