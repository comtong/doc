Vue使用原生高德SDK

1. public中的index.html上引入
	<!--引入高德地图JSAPI -->
	<script src="https://webapi.amap.com/maps?v=1.4.15&key=您申请的key值"></script>
2. 在vue.config.js中
    module.exports = {
    	configureWebpack: {
    	   externals: {
    	     'AMap': 'AMap'
    	   }
    	}
    }
3. 在需要引入map的布局上加上<div id="mapContainer"></div>
   在script中引入
   import AMap from "AMap"
   在mounted上添加
   let map = new AMap.Map("mapContainer", {
   		resizeEnable: true,
   		zoom: 10
   })
   // 引入插件方式
   AMap.plugin(["AMap.Autocomplete"], () => {
     var autoOptions = {
       city: "全国",
       input: "mapinput" // 这个是自动补全的input输入框的ID
     }
     this.amapAutoComPlugin = new AMap.Autocomplete(autoOptions)
   })

   注意：
   1. 上述任何配置均可在高德JS的API上查看具体信息
   2. 在mounted中没在created中是因为必须要dom渲染后才能拿到ID，另外如果是element-ui中的dialog，需要在updated中，因为dialog是懒加载