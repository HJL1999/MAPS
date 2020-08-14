<template>
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
          lngLats:[],
      }
  },
  mounted: function () {
    AMapLoader.load({
      "key": "c456a66f90bbed06c6214bc9d5168fcb", 
      "version": "1.4.15",   
      "plugins": []      
    }).then((AMap)=>{
      this.AMap=AMap;
          var map = new AMap.Map('container',{
          center:[116.397428, 39.90923],
          zoom:11,
      });
      this.map=map
    }).catch(e => {
        console.log(e);
    })

     
  },
  methods:{
    readFile(file){
        if (window.FileReader && file) {
          console.log(file)
          let reader = new FileReader();
          reader.onload = (e) => {
          console.log(e.target.result);
          this.fileContent = e.target.result;
          let tempData=JSON.parse(this.fileContent)
          if(tempData instanceof Array && tempData.length>0
            && tempData[0]['Longitude']&& tempData[0]['Latitude']){
            console.log('if')
            this.lnglats=tempData
            this.drawPath()
          }
          else{
            console.log('else')
          }
        };
      reader.readAsText(file.raw);
      }
    },
    drawPath(){
      let gps=[]
      console.log(this.lnglats)
      for(let e of this.lnglats){
        gps.push(new this.AMap.LngLat(e['Longitude'],e['Latitude']))
      }
      console.log(gps)
      this.AMap.convertFrom(gps, 'gps',(status, result)=> {
        if (result.info === 'ok') {
          let lnglats = result.locations; 
          console.log(lnglats)
      
          let polyline = new this.AMap.Polyline({
            path: lnglats,  
            borderWeight: 2, 
            strokeColor: 'blue', 
            lineJoin: 'round' 
          });

          
          this.map.add(polyline);
          this.map.setCenter(lnglats[1100]);
        }
        
      });
    }
  },
}

</script>
<style scoped>
  h3{
    margin: 40px 0 0;
  }
  ul{
    list-style-type: none;
    padding: 0;
  }
  li{
    display: inline-block;
    margin: 0 20px;
  }
  a{
    color:greenyellow;
  }
  textarea{
    width: 60vw;
    height: 80vh;
    position: fixed;
  }
  #container {
    width: 500px;
    height: 250px;
    position:fixed;
  }
</style>>

