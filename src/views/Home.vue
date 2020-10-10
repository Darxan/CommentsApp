<template>
  <div class="home">
    <CommentsPage msg="Гостевая книга"/>
  </div>
</template>

<script>
// @ is an alias to /src
import CommentsPage from './CommentsPage.vue'

export default {
  name: 'Home',
  components: {
    CommentsPage
  }
}
</script>
