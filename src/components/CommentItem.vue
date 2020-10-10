<template>
   <el-card
      :body-style="{ padding: '0px' }"
      class="comment__card"
      shadow="hover"
    >
    <img :src="data.img" :alt="'item-'+ data.id" width="100" height="auto" />
      <div class="comment__description">
        <h1>Коментария {{ data.id + 1 }}</h1>
        <h4>Имя: {{ data.name }}</h4>
        <strong>Коментария</strong> <span>: {{ data.comment }}</span>
        <div class="bottom clearfix">
          <time class="time"><strong>Дата добавления:</strong> {{ currentDate }}</time>
        </div>
      </div>
    </el-card>
</template>

<script>
  export default {
    name: "CommentItem",
    props: {
      data: {type: Object, required:true},
    },
    computed: {
    currentDate() {
      const year = new Date().getFullYear();
      const month = new Date().getMonth();
      const day = new Date().getDate()
      const timeR = new Date().toLocaleTimeString()
      const result = year.toString() + '/' +  month.toString() + '/' + day.toString() + '/' + timeR
      return result 
    }
  },
  }
</script>

<style lang="scss" scoped>

</style>