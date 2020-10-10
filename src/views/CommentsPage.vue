<template>
  <div class="hello">
    <h1>{{ title }}</h1>
    <el-container class="container">
      <el-row :gutter="20" class="comment">
        <el-col
          :span="windowWidth > 1024 ? 8 : 12"
          v-for="comment in comments"
          :key="comment.id"
        >
         <CommentItem :data="comment" />
        </el-col>
      </el-row>
    </el-container>
    <el-button class="button" @click="checkPost">Click me</el-button>
    <el-button  class="button" @click="dialogFormVisible = !dialogFormVisible">Add Comment</el-button>
    <el-dialog title="Add a new comment."  :width="windowWidth > 1024 ? '30%': '90%'" :visible.sync="dialogFormVisible">
      <el-form :model="form" @submit.prevent="uploadComment" ref="form">
        <el-form-item label="Your name">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="Your comment">
          <el-input v-model="form.comment"></el-input>
        </el-form-item>

      <el-upload
        action="https://jsonplaceholder.typicode.com/posts/"
        list-type="picture-card"
        :on-preview="handlePictureCardPreview"
        :on-remove="handleRemove"
        :on-success="addNewComment"
        :auto-upload="false"
        :on-error="handleError"
        :thumbnail-mode='true'
        ref="upload">
        <em class="el-icon-plus"></em>
        
      </el-upload>
      
      <el-dialog :visible.sync="dialogVisible">
        <img width="100%" :src="dialogImageUrl" alt="">
      </el-dialog>
      </el-form>
        <span slot="footer" class="dialog-footer">
          <el-button @click="dialogFormVisible = false">Cancel</el-button>
          <el-button type="primary" @click="uploadComment">Confirm</el-button>
        </span>
    </el-dialog>
  </div>
</template>

<script>
import CommentItem from '@/components/CommentItem.vue'
export default {
  name: "CommentsPage",
  props: {
    title: String,
  },
  data() {
    return {
      dialogFormVisible: false,
      windowWidth: 0,
      commentData: [],
      form: {
        name: '', 
        comment: '',
        img: null
      },
      dialogImageUrl: '',
      disabled: false,
      dialogVisible: false,
      comments: [
        {
          id:1, 
          name: 'Ivan',  
          'comment': 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Possimus eum saepe maxime.', 
          img: 'https://image.shutterstock.com/image-photo/man-holding-yellow-balloon-smile-260nw-519480580.jpg'
        },
        {
          id:2, 
          name: 'Ivan',  
          comment: 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Possimus eum saepe maxime.', 
          img: 'https://image.shutterstock.com/image-photo/man-holding-yellow-balloon-smile-260nw-519480580.jpg'
        },
        {
          id:3, 
          name: 'Ivan',  
          'comment': 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Possimus eum saepe maxime.', 
          img: 'https://image.shutterstock.com/image-photo/man-holding-yellow-balloon-smile-260nw-519480580.jpg'
        },
        
      ]
    };
  },
  created() {
    window.addEventListener('resize', this.handleResize);
    this.handleResize();
    this.$axios
      .get("https://jsonplaceholder.typicode.com/comments?postId=1")
      .then((response) => {
        this.commentData = response.data;
      })
      .catch((error) => {
        alert(error);
      });
  },
  destroyed() {
      window.removeEventListener('resize', this.handleResize);
  },
  components: {
    CommentItem,
  },
  methods: {
    checkPost() {
      const newdata = this.commentsData.slice(0,3);
      console.log("tag", newdata);
      for (let i = 0; i < newdata.length; i++) {
        console.log('tag', this.commentsData.length + i)
        newdata[i].id = this.commentsData.length + i
        console.log("date", newdata[i].id);
        this.commentsData.push(newdata[i]);
      }
      this.$message({
          message: "Success",
          type: "success",
          showClose: true,
        });
    },
    handleRemove(file, fileList) {
        console.log(file);
        console.log(fileList);
    },
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url
      this.dialogVisible = true;
    },
    addNewComment(file, fileList){
      const uid = this.comments.length + 1
      const image = fileList.url
      this.dialogImageUrl = image
      this.form.img = image 
      const newComment = {
          id: uid,
          name: this.form.name,
          comment: this.form.comment,
          img: fileList.url
      }
      this.comments.push(newComment)
      this.form.name = ''
      this.form.comment = ''
      this.form.img = null
      this.dialogFormVisible = false
      this.$message({
        message: "Your comment is successfuly uploaded!",
        type: "success",
        showClose: true,
      });
    },
    uploadComment(form){
      this.$refs.upload.submit();
    },
    handleDownload(file) {
      console.log(file);
    },
    handleError(err){
      if(err.isTrusted) {
        this.$message({
          message: "jpg/png files with a size less than 500kb",
          type: "error",
          showClose: true,
        });
      }
    },
    handleResize() {
          this.windowWidth = window.innerWidth;
    }
  },
};
</script>
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
}
.comment {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  &__card {
    margin: 10px 0px;
    padding: 10px;
  }&__description{
    text-align: left;
    padding: 15px 10px;
  }
}

</style>
