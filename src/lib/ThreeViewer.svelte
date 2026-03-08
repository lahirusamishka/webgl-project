<script>
	import { onMount } from 'svelte';
	import * as THREE from 'three';
	import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';
	import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';

	export let model;

	let container;

	onMount(() => {
		const scene = new THREE.Scene();

		const camera = new THREE.PerspectiveCamera(
			75,
			window.innerWidth / window.innerHeight,
			0.1,
			1000
		);

		const renderer = new THREE.WebGLRenderer({ antialias: true });
		renderer.setSize(window.innerWidth, window.innerHeight);

		container.appendChild(renderer.domElement);

		const light = new THREE.HemisphereLight(0xffffff, 0x444444);
		scene.add(light);

		camera.position.set(0, 1, 5);

		const controls = new OrbitControls(camera, renderer.domElement);
		controls.enableDamping = true;

		const loader = new GLTFLoader();

		loader.load(model, (gltf) => {
			scene.add(gltf.scene);
		});

		function animate() {
			requestAnimationFrame(animate);
			controls.update();
			renderer.render(scene, camera);
		}

		animate();
	});
</script>

<div bind:this={container}></div>
