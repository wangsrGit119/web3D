
#### 下载

```
官方网站:
---- https://photo-sphere-viewer.js.org
附属依赖必选项：
---- three.js
---- browser.js
注： 可以直接使用 npm install photo-sphere-viewer或者yarn add photo-sphere-viewer安装，会自动下载three.js和browser.js
```
官网demo详解：
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
<!--所有基础依赖引入-->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/photo-sphere-viewer@4/dist/photo-sphere-viewer.min.css"/>
<script src="https://cdn.jsdelivr.net/npm/three/build/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/uevent@2/browser.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/photo-sphere-viewer@4/dist/photo-sphere-viewer.min.js"></script>
<body>
<!--初始容器位置-->
<div id="viewer"></div>
<!--容器css设置，在网页中的大小等等设置-->
<style>
  #viewer {
    width: 100vw;
    height: 50vh;
  }
</style>
<!--最关键的一步，全景加载渲染的地方-->
<script>
  var viewer = new PhotoSphereViewer.Viewer({
   //需要渲染到的位置，上面div中定义的id，可以直接用 container: 'viewer'
    container: document.querySelector('#viewer'),
	//你的全景图片，可以直接是你自己手机拍的照片，但是记住这个文件要放在自己服务器，通过http或者https访问，不然会出现跨域问题
    panorama: 'https://photo-sphere-viewer.js.org/assets/Bryce-Canyon-National-Park-Mark-Doliner.jpg'
  });
</script>
</body>
</html>
```
> 好了，以上就可以实现一个简单的全景图片渲染demo了，你可以用手机拍自己周围，让别人足不出户访问你周围。

看看效果：
[图片上传失败...(image-fc368c-1589101404630)]
#### 自己动手实现
1.如果找不到下载依赖的地方，或者不想自己npm安装，可以直接将上面引入链接在浏览器打开，然后鼠标邮件另存为文件的名字，保存到本地，然后引入就可以。
2.准备图片，全景图片弄好后上传到随便可以用http访问的区域网或者公网，然后本地直接用链接

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>全景图渲染</title>
    <meta name="renderer" content="webkit">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=0">
    <meta http-equiv="Pragma" content="no-cache">
    <meta name="applicable-device" content="mobile">
	<!--下载到本地的基础依赖-->
    <link rel="stylesheet" href="./js/photo-sphere-viewer.min.css"/>
	<script src="./js/three.min.js"></script>
	<script src="./js/browser.min.js"></script>
	<script src="./js/photo-sphere-viewer.min.js"></script>
	<script src="./js/plugins/markers.js"></script>
<style>
<!--容器CSS-->
 #container {
    margin-top:10vh;
    width: 100vw;
    height: 70vh;
  }
</style>
</head>
<body>
<!--容器-->
    <div id="container"></div>
	<script >
		window.onload=function(){
			init();
		}
		//全景图参数配置函数
		function init(){
			var viewer = new PhotoSphereViewer.Viewer({
			//全景图位置
			container: 'container',
			//默认就是true
			navbar: true,
			//全景图自动开始旋转之前的空闲时间（毫秒）
			autorotateDelay: false,
			//旋转速度
			autorotateSpeed: '3rpm',
			//导航栏右边的提示问题
		    caption: '演示',
			defaultLat: 0.3,
			panorama: 'https://wangsr-oss-files.oss-cn-beijing.aliyuncs.com/images/%E5%85%A8%E6%99%AF/Bryce-Canyon-By-Jess-Beauchemin.jpg',
		    plugins: [
				PhotoSphereViewer.AutorotateKeypointsPlugin,
				PhotoSphereViewer.MarkersPlugin,
			  ],
		  });
		  //插件 点击则生成一个位置
		  var markersPlugin = viewer.getPlugin(PhotoSphereViewer.MarkersPlugin);
		  viewer.on('click', function(e, data) {
	      console.log(data)
		  if (!data.rightclick) {
			markersPlugin.addMarker({
			  id: '#' + Math.random(),
			  longitude: data.longitude,
			  latitude: data.latitude,
			  html: '&hearts;',
			  width: 40,
			  height: 40,
			  anchor: 'bottom center',
			  tooltip: '随机点',
			  data: {
				generated: true
			  }
			});
		  }
		}); 
		//双击取消生成的点
		markersPlugin.on('select-marker', function(e, marker, data) {
		  if (marker.data && marker.data.generated) {
			if (data.dblclick) {
			  markersPlugin.removeMarker(marker);
			} else if (data.rightclick) {
			  markersPlugin.updateMarker({
				id: marker.id,
				image: 'https://photo-sphere-viewer.js.org/assets/pin-blue.png',
			  });
			}
		  }
		})
		}
	</script>
</body>
</html>
```
> 好了，至此一个可以生成标记取消标记的demo完成了

[图片上传失败...(image-84582d-1589101404630)]
#### 总结
如果单纯的用three.js实现这些可能就没这么容易了，前面几个文章讲了一些基本的three.js的基础知识，完成web端3D的或者全景图的渲染了，你需要考虑很多东西去实现，而Photo Sphere Viewer不一样，只需要简单的几步，就可以实现大部分需求，定制化的也可以由自己开发。