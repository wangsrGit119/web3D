
#### ����

```
�ٷ���վ:
---- https://photo-sphere-viewer.js.org
����������ѡ�
---- three.js
---- browser.js
ע�� ����ֱ��ʹ�� npm install photo-sphere-viewer����yarn add photo-sphere-viewer��װ�����Զ�����three.js��browser.js
```
����demo��⣺
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
<!--���л�����������-->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/photo-sphere-viewer@4/dist/photo-sphere-viewer.min.css"/>
<script src="https://cdn.jsdelivr.net/npm/three/build/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/uevent@2/browser.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/photo-sphere-viewer@4/dist/photo-sphere-viewer.min.js"></script>
<body>
<!--��ʼ����λ��-->
<div id="viewer"></div>
<!--����css���ã�����ҳ�еĴ�С�ȵ�����-->
<style>
  #viewer {
    width: 100vw;
    height: 50vh;
  }
</style>
<!--��ؼ���һ����ȫ��������Ⱦ�ĵط�-->
<script>
  var viewer = new PhotoSphereViewer.Viewer({
   //��Ҫ��Ⱦ����λ�ã�����div�ж����id������ֱ���� container: 'viewer'
    container: document.querySelector('#viewer'),
	//���ȫ��ͼƬ������ֱ�������Լ��ֻ��ĵ���Ƭ�����Ǽ�ס����ļ�Ҫ�����Լ���������ͨ��http����https���ʣ���Ȼ����ֿ�������
    panorama: 'https://photo-sphere-viewer.js.org/assets/Bryce-Canyon-National-Park-Mark-Doliner.jpg'
  });
</script>
</body>
</html>
```
> ���ˣ����ϾͿ���ʵ��һ���򵥵�ȫ��ͼƬ��Ⱦdemo�ˣ���������ֻ����Լ���Χ���ñ����㲻������������Χ��

����Ч����
[ͼƬ�ϴ�ʧ��...(image-fc368c-1589101404630)]
#### �Լ�����ʵ��
1.����Ҳ������������ĵط������߲����Լ�npm��װ������ֱ�ӽ���������������������򿪣�Ȼ������ʼ����Ϊ�ļ������֣����浽���أ�Ȼ������Ϳ��ԡ�
2.׼��ͼƬ��ȫ��ͼƬŪ�ú��ϴ�����������http���ʵ����������߹�����Ȼ�󱾵�ֱ��������

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>ȫ��ͼ��Ⱦ</title>
    <meta name="renderer" content="webkit">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=0">
    <meta http-equiv="Pragma" content="no-cache">
    <meta name="applicable-device" content="mobile">
	<!--���ص����صĻ�������-->
    <link rel="stylesheet" href="./js/photo-sphere-viewer.min.css"/>
	<script src="./js/three.min.js"></script>
	<script src="./js/browser.min.js"></script>
	<script src="./js/photo-sphere-viewer.min.js"></script>
	<script src="./js/plugins/markers.js"></script>
<style>
<!--����CSS-->
 #container {
    margin-top:10vh;
    width: 100vw;
    height: 70vh;
  }
</style>
</head>
<body>
<!--����-->
    <div id="container"></div>
	<script >
		window.onload=function(){
			init();
		}
		//ȫ��ͼ�������ú���
		function init(){
			var viewer = new PhotoSphereViewer.Viewer({
			//ȫ��ͼλ��
			container: 'container',
			//Ĭ�Ͼ���true
			navbar: true,
			//ȫ��ͼ�Զ���ʼ��ת֮ǰ�Ŀ���ʱ�䣨���룩
			autorotateDelay: false,
			//��ת�ٶ�
			autorotateSpeed: '3rpm',
			//�������ұߵ���ʾ����
		    caption: '��ʾ',
			defaultLat: 0.3,
			panorama: 'https://wangsr-oss-files.oss-cn-beijing.aliyuncs.com/images/%E5%85%A8%E6%99%AF/Bryce-Canyon-By-Jess-Beauchemin.jpg',
		    plugins: [
				PhotoSphereViewer.AutorotateKeypointsPlugin,
				PhotoSphereViewer.MarkersPlugin,
			  ],
		  });
		  //��� ���������һ��λ��
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
			  tooltip: '�����',
			  data: {
				generated: true
			  }
			});
		  }
		}); 
		//˫��ȡ�����ɵĵ�
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
> ���ˣ�����һ���������ɱ��ȡ����ǵ�demo�����

[ͼƬ�ϴ�ʧ��...(image-84582d-1589101404630)]
#### �ܽ�
�����������three.jsʵ����Щ���ܾ�û��ô�����ˣ�ǰ�漸�����½���һЩ������three.js�Ļ���֪ʶ�����web��3D�Ļ���ȫ��ͼ����Ⱦ�ˣ�����Ҫ���Ǻܶණ��ȥʵ�֣���Photo Sphere Viewer��һ����ֻ��Ҫ�򵥵ļ������Ϳ���ʵ�ִ󲿷����󣬶��ƻ���Ҳ�������Լ�������