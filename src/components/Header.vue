<template>
  <div class="header">
    <div class="content">
      <div class="container clearfix">
      <div class="title">
        TodoList
      </div>
      <div class="header-search">
        <input type="text" placeholder="添加ToDo" v-model="title" @keyup.enter="handleadd" >
        <button @click="handleadd">ADD</button>
      </div>
      </div>
    </div>
  </div>

</template>

<script>
  import axios from 'axios'
    export default {
        name: "Header",
      props: {
        getData: {
          type: Function
        },
      },
        data() {
          return {
            title: "",
          }
        },
        methods: {

          handleadd() {
            // console.log(this.title)
            if (!this.title.trim()) {
              alert("请输入todo后在添加")
            }
            else {
              let title = this.title;
              console.log(title)
              axios.post('/api', {title,}).then(res => {
                if (res.data.code == 200) {
                  alert("添加成功")
                  this.title = "";
                  this.getData();
                }
                else {
                  alert("添加失败")
                }
              })
            }
          },

        },
      }
</script>

<style scoped>
  .clearfix:after{
    content:"";
    display: block;
    clear: both;
  }
  .content{
    height: 52px;
    background: #323232;
    position: relative;
  }

  .container{
     width: 600px;
     height: 30px;
     color: #fff;
     background: #323232;
    position: absolute;
    top: 50%;
    left: 50%;
    margin-top: -15px;
    margin-left: -300px;
  }
  .title{
    float: left;
    font-size: 20px;

  }
  .header-search{
    float: right;
    width: 450px;
  }
  .header-search input{
    width: 300px;
    height: 30px;
    color: #ccc;
    border-radius: 6px;
  }
  .header-search button{
    color: #fff;
    width: 100px;
    height: 30px;
    background: #ccc;
    border: 1px solid #ccc;
    border-radius: 8px;
  }
</style>
