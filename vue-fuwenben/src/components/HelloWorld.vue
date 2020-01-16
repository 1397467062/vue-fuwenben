<template>
  <div >
    <ul>
    <draggable :options="{group:'people',animation:150,ghostClass:'sortable-ghost',chosenClass:'chosenClass',scroll:true,scrollSensitivity:200}"
               v-model="list2"
               @change="change"
               @start="start"
               @end="end"
               :move="move"
    >
      <div class="list" v-for= "(item,index) in list2">
        <div v-if="item.name" style="position: relative">
          <i @click="del(index)" style="position: absolute;right: 0" class="el-icon-circle-close"></i>
          <span >{{item.name}}</span>
        </div>
        <div v-else style="width: 100%;height: 100%;position: relative; overflow: hidden" >
          <i @click="del(index)" style="position: absolute;right: 0" class="el-icon-circle-close"></i>

          <img style="width: 100%;"  :src="item.src">
        </div>
      </div>
    </draggable>
    </ul>
   <!--  菜单  -->
    <el-row :gutter="20" class="menu">
      <el-col :span="6" >
        <div @click="dialogVisible = true">
          <i class="el-icon-edit" ></i>  文字
        </div>
      </el-col>
      <el-col :span="6">
        <el-upload
          class="avatar-uploader"

          :show-file-list="false"
           action="#"
          :on-change=" beforeAvatarUpload"

         >
<!--          <img v-if="imageUrl" :src="imageUrl" class="avatar">-->
           <i class="el-icon-camera"></i>  照片
        </el-upload>

      </el-col>
      <el-col :span="6">
        <div @click="show=true">  <i class="el-icon-camera"></i> 预览</div>

      </el-col>
      <el-col :span="6"><div class="grid-content bg-purple"></div></el-col>
    </el-row>

<!--  对话  -->
    <el-dialog title="提示" :visible.sync="dialogVisible" :before-close="handleClose" width="100%">
      <el-input v-model="input" placeholder="请输入内容"></el-input>
      <span slot="footer" class="dialog-footer">
         <el-button type="primary" @click="textOk">确 定</el-button>
     </span>
    </el-dialog>

    <el-dialog  :fullscreen="true" title="预览" :visible.sync="show" :before-close="handleClose" width="100%">
        <div v-for= "item in list2">
          <span v-if="item.name">{{item.name}}</span>
          <img style="width: 100%;" v-else :src="item.src">
        </div>
    </el-dialog>

  </div>
</template>

<script>
  import draggable from "vuedraggable"
export default {
  name: 'HelloWorld',
  data () {
    return {
      input:"",
      msg: 'Welcome to Your Vue.js App',
      show:false,
      imageUrl:"",
      dialogVisible:false,
      list2:[
        {name:1},
        {name:12123}
      ]
    }
  },
  components: {draggable},
  methods:{
    del(index){
       console.log(index)
      this. list2.splice(index,1)
    },
    beforeAvatarUpload(file,fileList){
      console.log(file)
      console.log(fileList)
      var _this = this;
      var event = event || window.event;
      var file = event.target.files[0];
      var reader = new FileReader();
      //转base64
      reader.onload = function(e) {
        _this.imageUrl = e.target.result //将图片路径赋值给src
        _this.list2.push({
          src: e.target.result
        })
      }
      reader.readAsDataURL(file);

    },
    handleAvatarSuccess(e){
      console.log(e)
    },
    textOk:function(){
      this.list2.push({
        name:this.input
      })
      this.  dialogVisible=false
    },
    handleClose:function(){
      this.dialogVisible=false
      this.show=false
    },
    //evt里面有两个值，一个evt.added 和evt.removed  可以分别知道移动元素的ID和删除元素的ID
    change: function (evt) {
      console.log(evt)
    },
    //start ,end ,add,update, sort, remove 得到的都差不多
    start: function (evt) {
      // console.log(evt)
    },
    end: function (evt) {
      // console.log(evt)
      // evt.item //可以知道拖动的本身
      // evt.to    // 可以知道拖动的目标列表
      // evt.from  // 可以知道之前的列表
      // evt.oldIndex  // 可以知道拖动前的位置
      // evt.newIndex  // 可以知道拖动后的位置
      console.log(this.list2)
    },
    move: function (evt, originalEvent) {

      // console.log(originalEvent) //鼠标位置
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
 .menu{
   box-shadow:1px 0px 5px rgba(000,000,000,0.3);
 }
h1, h2 {
  font-weight: normal;
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
.list{
  height: 50px;
  border: 1px solid #ddd ;
  margin-bottom: 20px;
}
</style>
