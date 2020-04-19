

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
   
    <style type="text/css">
            div#canvas-frame {
                border: none;
                cursor: pointer;
                width: 100%;
                height: 900px;
                background-color: white;
            }

        </style>
    <script src="./js/three.js"></script>
      <!-- 常用相机控制器，依赖文件目录 three.js\examples\js\controls -->
   <script  src="./js/controls/OrbitControls.js"></script>
    <!-- dat.gui 控制js变量 -->
     <script  src="./js/gui/dat.gui.js"></script>
</head>
<body>
    <body onload="threeStart();">
        <div id="canvas-frame"></div>
    </body>

    <script>
        var axesHelper;
        var renderer;
        var scene;
        var camera;
        var orbitcontrols;
        var gui;
        init();
        function init(){
            //init axesHelper
            axesHelper = new THREE.AxesHelper( 10000 );
            axesHelper.position.y = 0;
            // init renderer
            let width = document.getElementById('canvas-frame').clientWidth;
            let height = document.getElementById('canvas-frame').clientHeight;
            renderer = new THREE.WebGLRenderer({
                antialias : true
            });
            renderer.setSize(width, height);
            document.getElementById('canvas-frame').appendChild(renderer.domElement);
            renderer.setClearColor(0xFFFFFF, 1.0);
            // init scene
            scene = new THREE.Scene();
            scene.background = new THREE.Color(0XFFFFFF);
            scene.add( axesHelper);
            //init camera
            camera = new THREE.PerspectiveCamera(45, width / height, 1, 10000);
            camera.position.set(100,140,100);
            camera.up.set(0,1,0)
            camera.lookAt(0, 0, 0)
            //init orbitcontrols;
            orbitcontrols = new THREE.OrbitControls(camera,renderer.domElement);
            orbitcontrols.rotateSpeed = -0.25;
            // //gui 初始化
            gui = new dat.GUI();
            gui.add(orbitcontrols, 'rotateSpeed');
            initObject() ;
        }
      
      


        function initObject() {
            const url = "https://wangsr-oss-files.oss-cn-beijing.aliyuncs.com/images/%E5%85%A8%E6%99%AF/roberto-nickson-rEJxpBskj3Q-unsplash.jpg";
            var geometry = new THREE.SphereGeometry(50, 200, 200); 
            var material;
           // 初始化一个加载器
                var loader = new THREE.TextureLoader();

                // 加载一个资源
                loader.load(
                    // 资源URL
                    url,
                    // onLoad回调
                    function ( texture ) {
                        // in this example we create the material when the texture is loaded
                         material = new THREE.MeshBasicMaterial( { 
                            map: texture
                        } );
                         //双面渲染
                         material.side = THREE.DoubleSide;

                         var mesh = new THREE.Mesh(geometry, material); //网格模型对象Mesh
                         scene.add(mesh); 
                          //点光源
                          var spotLight = new THREE.SpotLight(0xffffff);
                          spotLight.position.set(-40, 60, -10);
                          spotLight.castShadow = true;
                          scene.add(spotLight);
                    },
                   undefined,
                    // onError回调
                    function ( err ) {
                        console.error( 'An error happened.' );
                    }
                );   
           
     }

        
        function threeStart() {
                renderer.clear();
                requestAnimationFrame( threeStart );
                orbitcontrols.update(); // required when damping is enabled
                renderer.render( scene, camera );
            }
        
    </script>

</body>
</html>
```