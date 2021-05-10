<template>
  <div class="car-container" ref="canvas">
  </div>
</template>

<script>
import * as Three from 'three'
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
export default {
    name: "car-model",
    data() {
        return {
            camera: null,
            scene: null,
            renderer: null,
            loader: null,
            controls: null,
            light: null,
            secondLight: null,
            rmapped: 5
        }
    },
    mounted() {
        this.init()
    },
    methods: {
        init() {
            this.renderer = new Three.WebGLRenderer({
                antialias: true
            });
            this.$refs.canvas.appendChild(this.renderer.domElement);

            this.camera = new Three.PerspectiveCamera(45, 2, 0.1, 100);
            this.camera.position.set(0, 0.5, 5);

            this.controls = new OrbitControls(this.camera, this.renderer.domElement);
            this.controls.update();
            this.controls.maxPolarAngle = Math.PI * 2 / 4;
            this.controls.autoRotate = true;
            this.controls.autoRotateSpeed = 3.0;
            this.controls.enableZoom = false;
            this.controls.enableRotate = false;

            this.scene = new Three.Scene();
            this.scene.background = new Three.Color('rgb(15, 15, 15)');

            let lightColor = "hsl(35, 50%, 50%)";
            let intensity = 2.5;
            this.light = new Three.DirectionalLight(lightColor, intensity);
            this.light.position.set(3, 3, 1);
            this.light.target.position.set(0, 1, 0);
            
            this.scene.add(this.light);
            this.scene.add(this.light.target)
            
            this.secondLight = new Three.DirectionalLight(lightColor, .5);
            this.secondLight.position.set(0, 5, -0.5);
            this.secondLight.target.position.set(0, 3, 1)
            
            this.scene.add(this.secondLight);
            this.scene.add(this.secondLight.target);

            let light1 = new Three.PointLight("rgb(230, 255, 250)", 4);
            light1.position.set(0, -10, 0);
            
            this.scene.add(light1);

            const gltfLoader = new GLTFLoader();
            const url = location.origin + "/car/scene.gltf";
            gltfLoader.load(url, (gltf) => {
                const root = gltf.scene;
                this.scene.add(root);
                const box = new Three.Box3().setFromObject(root);
                const boxCenter = box.getCenter(new Three.Vector3());
                this.controls.target.copy(boxCenter);
                this.controls.update();
            })
            requestAnimationFrame(this.render);
            setInterval(this.changeLightColor, 3000);
        },
        changeLightColor() {
            let h = this.rmapped * 0.01 % 1;
            let s = 0.5;
            let l = 0.5;
            this.light.color.setHSL( h, s, l );
            this.rmapped++;
            this.light.intensity = (Math.random() * (3 - 1)) + 1;
            this.secondLight.color.setHSL(h, s, 0.4);
        },
        render() {
            if (this.resizeRendererToDisplaySize(this.renderer)) {
                const canvas = this.renderer.domElement;
                this.camera.aspect = canvas.clientWidth / canvas.clientHeight;
                this.camera.updateProjectionMatrix();
            }
            this.controls.update();
            this.renderer.render(this.scene, this.camera);
            requestAnimationFrame(this.render);
        },
        resizeRendererToDisplaySize() {
            const canvas = this.renderer.domElement;
            const width = canvas.clientWidth;
            const height = canvas.clientHeight;
            const needResize = canvas.width !== width || canvas.height !== height;
            if (needResize) {
                this.renderer.setSize(width, height, false);
            }
            return needResize;
        },
    }
}
</script>

<style style-scoped lang='scss'>
.car-container {
    canvas {
        width: 100%;
        height: 100%;
    }
}
</style>