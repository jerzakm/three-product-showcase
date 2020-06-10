<script>
  import * as Stats from "stats.js";
  import * as THREE from 'three';

  //FPS counter
  const stats = new Stats();
  stats.showPanel(0); // 0: fps, 1: ms, 2: mb, 3+: custom
  document.body.appendChild(stats.dom);


  import {
    OrbitControls
  } from 'three/examples/jsm/controls/OrbitControls.js';
  import {
    GLTFLoader
  } from 'three/examples/jsm/loaders/GLTFLoader.js';
  import {
    RGBELoader
  } from 'three/examples/jsm/loaders/RGBELoader.js';
  import {
    RoughnessMipmapper
  } from 'three/examples/jsm/utils/RoughnessMipmapper.js';

  var container, controls;
  var camera, scene, renderer;

  init();
  render();

  function init() {

    container = document.createElement('div');
    document.body.appendChild(container);

    camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.25, 20);
    camera.position.set(-3, 3, 4);
    camera.far = 5000

    scene = new THREE.Scene();

    new RGBELoader()
      .setDataType(THREE.UnsignedByteType)
      .load('royal_esplanade_1k.hdr', function (texture) {

        var envMap = pmremGenerator.fromEquirectangular(texture).texture;

        // scene.background = envMap;
        scene.environment = envMap;

        texture.dispose();
        pmremGenerator.dispose();

        render();

        // var light = new THREE.AmbientLight(0x404040); // soft white light
        // scene.add(light);

        // var light = new THREE.PointLight(0xeeeeee, 1, 500);
        // light.position.set(50, 50, 50);
        // scene.add(light);

        // var light = new THREE.PointLight(0xeeeeee, 1, 500);
        // light.position.set(0, 50, 50);
        // scene.add(light);

        // model

        // use of RoughnessMipmapper is optional
        var roughnessMipmapper = new RoughnessMipmapper(renderer);

        // const modelPath = 'DamagedHelmet/glTF/'
        // const modelFile = 'DamagedHelmet.gltf'

        const modelPath = 'dread/'
        const modelFile = 'scene.gltf'

        var loader = new GLTFLoader().setPath(modelPath);
        loader.load(modelFile, function (gltf) {

          console.log(gltf)

          gltf.scene.traverse(function (child) {

            if (child.isMesh) {

              roughnessMipmapper.generateMipmaps(child.material);

            }

          });
          gltf.scene.scale.set(0.01, 0.01, 0.01)

          scene.add(gltf.scene);

          roughnessMipmapper.dispose();

          render();

        });

      });

    renderer = new THREE.WebGLRenderer({
      antialias: true,
      alpha: true
    });
    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.toneMapping = THREE.ACESFilmicToneMapping;
    renderer.toneMappingExposure = 0.8;
    renderer.outputEncoding = THREE.sRGBEncoding;
    renderer.setClearColor(0xFFFFFF, 0)
    container.appendChild(renderer.domElement);
    renderer.domElement.style.position = 'fixed'

    var pmremGenerator = new THREE.PMREMGenerator(renderer);
    pmremGenerator.compileEquirectangularShader();

    controls = new OrbitControls(camera, renderer.domElement);
    controls.addEventListener('change', render); // use if there is no animation loop
    controls.minDistance = 0;
    controls.maxDistance = 80;
    controls.target.set(0, 0, -0.2);
    controls.update();

    window.addEventListener('resize', onWindowResize, false);

  }

  function onWindowResize() {

    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();

    renderer.setSize(window.innerWidth, window.innerHeight);

    render();

  }

  //

  function render() {

    renderer.render(scene, camera);

  }

  animate();
  requestAnimationFrame(animate);

  function animate() {
    requestAnimationFrame(animate);
    stats.begin();

    stats.end();
  }
</script>