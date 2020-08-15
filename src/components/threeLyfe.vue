<template>
  <div id="cube">
  </div>
</template>

<script>
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';

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
        scene.background = new THREE.Color( 0x777777 );

			  var camera = new THREE.PerspectiveCamera( 1, window.innerWidth / window.innerHeight, 0.1, 10000 );
        camera.position.set( 200, 200, 300);

			  var renderer = new THREE.WebGLRenderer();
        renderer.setSize( window.innerWidth, window.innerHeight );
        renderer.shadowMap.enabled = true;
        
			  document.querySelector("#cube").appendChild( renderer.domElement );
        
        // cube

        var geometry = new THREE.BoxGeometry();
        
			  var material = new THREE.MeshPhysicalMaterial( { 
          color: 0xffffff,
          transparent: true,
          transmission: 0.8,
         });

			  var cube = new THREE.Mesh( geometry, material );
        cube.position.set( 0, 0, 0 );
        cube.frustumCulled = false; 
			  scene.add( cube );

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

				light.shadow.camera.far = 1000;

				scene.add( light );

				// ground

        var groundMaterial = new THREE.MeshLambertMaterial( { color: 0x555555 } );

				var mesh = new THREE.Mesh( new THREE.PlaneBufferGeometry( 20000, 20000 ), groundMaterial );
				mesh.position.y = - 2;
				mesh.rotation.x = - Math.PI / 2;
				mesh.receiveShadow = true;
				scene.add( mesh );

        // controls
				var controls = new OrbitControls( camera, renderer.domElement );
				controls.maxPolarAngle = Math.PI * 0.5;
				controls.minDistance = 250;
				controls.maxDistance = 500;
          
        var animate = function () {
          requestAnimationFrame( animate );
          //cube.rotation.y += 0.01;
          renderer.render( scene, camera );
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