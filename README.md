# MAPS<template>
  <div class="hello">
 
    <el-upload
  class="upload-demo"
  action=""
  :on-change="readFile"
  :auto-upload="false"
  >
  <el-button size="small" type="primary">点击上传</el-button>
  <div slot="tip" class="el-upload__tip">只能上传路径文本文件</div>
   
</el-upload>
<div id="container"></div>
 <textarea disabled v-bind:value='fileContent' ></textarea>
  </div>
</template>

<script>
import AMapLoader from '@amap/amap-jsapi-loader';
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data:function(){
      return{
          fileContent:'文件读取内容...',
          map : null,
          AMap: null,
      }
  },
  mounted:function(){
AMapLoader.load({
    "key": "c456a66f90bbed06c6214bc9d5168fcb",              
    "version": "1.4.15",   
    "plugins": []           
}).then((AMap)=>{
  this.AMap=AMap;
    let map = new AMap.Map('container', {
    zoom:11,
    center: [116.397428, 39.90923],
    viewMode:'3D'
  
});
this.map=map;
}).catch(e => {
    console.log(e);
})
  },
  methods:{
      readFile(file){
          console.log(file)
    if (window.FileReader && file) {
        let reader = new FileReader();
        reader.onload = (e) => {
        console.log(e.target.result);
        this.fileContent = e.target.result;
        };
        reader.readAsText(file.raw);
        }
      },
  },
  }

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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
textarea{ 
    width: 60vw;
    height: 80vh;

}
#container{
  width:500px;
  height:300px;
}
</style>
