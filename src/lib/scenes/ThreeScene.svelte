<script lang="ts">
    import { onMount, onDestroy } from 'svelte';
    import * as THREE from 'three';

    let canvas: HTMLCanvasElement | null = null;
    let renderer: THREE.WebGLRenderer;
    let camera: THREE.PerspectiveCamera;
    let scene: THREE.Scene;
    let cube: THREE.Mesh;

    const initScene = () => {
        if (!canvas) return;

        scene = new THREE.Scene();
        camera = new THREE.PerspectiveCamera(75, canvas.clientWidth / canvas.clientHeight, 0.1, 1000);
        renderer = new THREE.WebGLRenderer({ canvas, antialias: true });

        renderer.setSize(canvas.clientWidth, canvas.clientHeight);
        renderer.setPixelRatio(window.devicePixelRatio);

        const geometry = new THREE.BoxGeometry();
        const material = new THREE.MeshStandardMaterial({ color: "#4CAF50" });
        cube = new THREE.Mesh(geometry, material);
        scene.add(cube);

        // Ambient lighting for better visualization
        const light = new THREE.AmbientLight(0xffffff, 1);
        scene.add(light);

        camera.position.z = 5;

        const animate = () => {
            requestAnimationFrame(animate);
            cube.rotation.x += 0.01;
            cube.rotation.y += 0.01;
            renderer.render(scene, camera);
        };

        animate();
    };

    onMount(() => {
        initScene();
        window.addEventListener("resize", () => {
            if (canvas && renderer && camera) {
                const width = canvas.clientWidth;
                const height = canvas.clientHeight;
                renderer.setSize(width, height);
                camera.aspect = width / height;
                camera.updateProjectionMatrix();
            }
        });
    });

    // onDestroy(() => {
    //     renderer.dispose();
    // });
</script>

<canvas bind:this={canvas} class="w-full h-full shadow-lg"></canvas>
