<template>
  <div id="cube">
  </div>
</template>

<script>
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';
import { RGBELoader } from 'three/examples/jsm/loaders/RGBELoader.js';

export default {
  name: 'three-lyfe',
  data () {
    return {}
  },
  
  methods: {
    createScene() {
      window.addEventListener('resize', () => {
          renderer.setSize(window.innerWidth, window.innerHeight);
          camera.aspect = window.innerWidth / window.innerHeight;
          camera.updateProjectionMatrix();
        });

        // camera & renderer
        var scene = new THREE.Scene();
				scene.fog = new THREE.Fog( 0xcccccc, 500, 10000 );
        scene.background = new THREE.Color( 0xffffff );

			  var camera = new THREE.PerspectiveCamera( 1, window.innerWidth / window.innerHeight, 0.1, 10000 );
        camera.position.set( 1000, 0, 700);

			  var renderer = new THREE.WebGLRenderer();
        renderer.setSize( window.innerWidth, window.innerHeight );
        renderer.shadowMap.enabled = true;
        
			  document.querySelector("#cube").appendChild( renderer.domElement );

        // cubes
        
        //var w = 2;
        //var geometry = new THREE.BoxGeometry(w, w, w);
        //
			  //var material = new THREE.MeshPhysicalMaterial({ 
        // color: 0xffffff,
        // transparent: true,
        // transmission: 0.8,
        // wireframe: false
        //});

        //var material1 = new THREE.MeshPhysicalMaterial({ 
        // color: 0x2576f7,
        // transparent: true,
        // transmission: 0.8,
        // wireframe: false
        //}); 

        //var cube = new THREE.Mesh( geometry, material );
        //cube.position.set( 0, 0, 0 );
        //cube.frustumCulled = false;
        //scene.add( cube );

        //var cube1 = new THREE.Mesh( geometry, material1 );
        //cube1.position.set( w, 0, 0 );
        //cube1.frustumCulled = false;
        //scene.add( cube1 );

        //dynamically generate cube

        //length = length/width/height of indiv. cubes, cubeNum = dimension

        //this function assumes the origin cube is (0,0,0), this can later be changed to be more dynamic
        //but this isn't neccessary for our purposes at the moment.
        var generate3dGrid = function(length, cubeNum) {

          if (cubeNum && length > 0) {
            var row = 0;
            var z = 0;
            //creation of the CubeKey used for creating rows
            if (cubeNum % 2 == 0) {
              var cubeNumLeft = (cubeNum/2) - 1;
              var cubeNumRight = cubeNum/2;
            } else {
              var cubeNumLeft = cubeNum/2;
              var cubeNumRight = cubeNum/2;
            }

            //Arrays that hold the Neg and Pos positions
            var cubeNegArr = [];
            var cubePosArr = [];
            //Combines both arrays with origin cube to create a key for creating the whole grid
            var cubeKey = [];

            for (let i = 0; i < cubeNumLeft; i++) {
              if (i == 0) {
                cubeNegArr[i] = -length;
              } else {
                cubeNegArr[i] = cubeNegArr[i-1] - length;
              }
              cubeKey.push(cubeNegArr[i]);
            }

            cubeKey.push(0);

            for (let i = 0; i < cubeNumRight; i++) {
              if (i == 0) {
                cubePosArr[i] = length; 
              } else {
                cubePosArr[i] = cubePosArr[i+1] + length; 
              }
              cubeKey.push(cubePosArr[i]);
            }

            var cube;
            if (cubeNum == 1) {
              cube = [];
            } else if (cubeNum == 2) {
              cube = [[],[]];
            } else if (cubeNum == 3) {
              cube = [[[],[],[]],[[],[],[]],[[],[],[]]];
            }

            var geometry = new THREE.BoxGeometry(length, length, length);

			      var material = new THREE.MeshPhysicalMaterial({ 
             color: 0xffffff,
             transparent: true,
             transmission: 0.8,
             wireframe: false
            });

            if (cubeNum == 3) {
              for (let i = 0; i < cubeNum; i++) {
                for (let j = 0; j < cubeNum; j++) {
                  for (let k = 0; k < cubeNum; k++) {
                    cube[i][j][k] = new THREE.Mesh( geometry, material );
                    cube[i][j][k].position.set( cubeKey[j], cubeKey[i], cubeKey[k] );
                    cube[i][j][k].frustumCulled = false; 
                    scene.add( cube[i][j][k]);
                    console.log("cube created.");
                  }
                }
              }
            }
          } else {
            return;
          }
        }

      //please don't break ty
      generate3dGrid(2, 3);

        // lights

				scene.add( new THREE.AmbientLight( 0x666666 ) );

				var light = new THREE.DirectionalLight( 0xdfebff, 1 );
				light.position.set( 50, 200, 100 );
				light.position.multiplyScalar( 1.3 );

				light.castShadow = true;

				light.shadow.mapSize.width = 1024;
				light.shadow.mapSize.height = 1024;

				var d = 300;

				light.shadow.camera.left = - d;
				light.shadow.camera.right = d;
				light.shadow.camera.top = d;
				light.shadow.camera.bottom = - d;

				light.shadow.camera.far = 10000;

				scene.add( light );

				// ground

        var groundMaterial = new THREE.MeshLambertMaterial( { color: 0x777777 } );

				var mesh = new THREE.Mesh( new THREE.PlaneBufferGeometry( 20000, 20000 ), groundMaterial );
				mesh.position.y = - 10;
				mesh.rotation.x = - Math.PI / 2;
				mesh.receiveShadow = true;
				scene.add( mesh );

        // controls
				var controls = new OrbitControls( camera, renderer.domElement );
				controls.maxPolarAngle = Math.PI * 0.5;
				controls.minDistance = 250;
				controls.maxDistance = 1000;
        controls.enableDamping = true;
        controls.dampingFactor = 0.03;
        controls.rotateSpeed = 0.17;


        var animate = function () {
          requestAnimationFrame( animate );
          renderer.render( scene, camera );
          controls.update();
			  };
			  animate();
    }
  },

  mounted () {
    this.createScene();
    }
  }
</script>

<style>
  html, body {
    margin: 0;
    padding: 0;
    overflow: hidden;
  }
</style>