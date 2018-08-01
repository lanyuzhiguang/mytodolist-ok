<template>
    <div class="home">
      <headerhome :getData="getData"></headerhome>
      <div class="home-content">
        <div class="doing ">
          <div class="doing-top clearfix">
            <div class="doing-title fll">
              <h2>正在进行</h2>
            </div>
            <div class="doingall flr">
             {{doingarr.length}}
            </div>
          </div>
          <div class="doing-content" >
            <div class="doing-item" v-for="item in doingarr">
                <input type="checkbox" class="doing-select fll" v-model="item.isDone"  @change="changeDone(item,item._id)" :getData="getData">
                <span >{{item.title}}</span>
              <button class="doing-btn flr"  @click="deleteData(item._id)">删除</button>
            </div>
          </div>
        </div>
        <div class="finish ">
          <div class="finish-top clearfix">
            <div class="finish-title fll">
              <h2>已经完成</h2>
            </div>
            <div class="finishall flr">
              {{finisharr.length}}
            </div>
          </div>
          <div class="finish-content">
            <div class="finish-item" v-for="fini in finisharr">
              <input type="checkbox" class="finish-select fll" v-model="fini.isDone" @change="changeDone(fini,fini._id)">
              <span >{{fini.title}}</span>
              <button class="finish-btn flr" @click="deleteData(fini._id)">删除</button>
            </div>
          </div>
        </div>
      </div>
    </div>
</template>
<script>
  import axios from 'axios'
  import headerhome from './components/Header'
    export default {
        name: "Home",
      components:{
        headerhome
      },
      getData: {
        type: Function
      },
      data(){
          return{
            doingarr:[

              ],
            finisharr:[

            ],
      }


      },
      methods:{

        getData(){
            axios.get( `/api`).then(res=>{
               var doingarr= res.data.data.filter(function (falseitem) {
                 // console.log(falseitem)
                 // console.log(falseitem.isDone)
                 return falseitem.isDone==false
               });
              this.doingarr=doingarr;
              var finisharr= res.data.data.filter(function (trueitem) {
                // console.log(trueitem.isDone)
                return trueitem.isDone==true;
              });
              this.finisharr=finisharr;
            })
        },

        deleteData(id){
          // console.log(id)
          axios.delete( `/api/${id}`, ).then(res=>{
            if(res.data.code==200){
              alert(  "删除成功")
              this.getData()
            }
            else{
              alert(  "删除失败")
            }
          });

        },
        changeDone(value,id){
          let isDone =value.isDone?1:0;
          console.log(value)
          // this.isDone=isDone
          console.log(id)
          console.log(isDone)
          axios.patch(`/api/${id}`,{isDone}).then(res=>{


            if(res.data.code==200){
              alert(  "状态修改成功")
              this.getData()
            }
            else{
              alert(  "状态修改失败")
            }
          })
        },


      },
      mounted () {
        this.getData();
      }
    }
</script>

<style scoped>

  .clearfix:after{
    content:"";
    display: block;
    clear: both;
  }
  .fll{
    float: left;
  }
  .flr{
    float: right;
  }
 .home-content{
   width: 600px;
   margin: 0 auto;
 }
 .doing,.finish{
   padding-top: 10px;
   padding-bottom: 10px;
 }
  .doingall,.finishall {
    line-height: 30px;
    text-align: center;
    width: 30px;
    height: 30px;
    background: #EAE3E3;
    color: #fff;
    border-radius: 50%;
  }
  .doing-content,.finish-content{
    padding-top: 20px;
    padding-bottom: 20px;
  }
  .doing-item,.finish-item{
    margin: 10px;
   padding: 0px 10px 0px 20px;
    width:565px;
    height: 30px;
    border-left:4px solid #1550CB ;
    border-radius: 4px;
    background: #fff;
    line-height: 30px;
  }

  .doing-select,.finish-select{
    width: 20px;
    height: 20px;
    margin-top: 5px;
  }
  .doing-btn,.finish-btn{
    margin-top: 5px;
  }




</style>
