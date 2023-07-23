<script lang="ts">
	import { browser } from '$app/environment';
	import * as Threlte from '@threlte/core';
	import * as Three from 'three';
	import * as Utils from 'three/src/math/MathUtils';
	import { Pane } from 'tweakpane';
	const gridHelper = new Three.GridHelper(20, 10);
	const axisHelper = new Three.AxesHelper(10);

	const sphere = {
		position: { x: 0, y: 4, z: 0 }
	};

	if (browser) {
		const pane = new Pane({ title: 'Scene' });

		const sphereControls = pane.addFolder({ title: 'Sphere' });
		sphereControls.addInput(sphere, 'position');

		sphereControls.on('change', ({ value }) => {
			sphere.position = value as Three.Vector3;
		});
	}
</script>

<Threlte.Canvas>
	<Threlte.Object3DInstance object={gridHelper} />
	<Threlte.Object3DInstance object={axisHelper} />

	<Threlte.PerspectiveCamera position={{ x: 20, y: 20, z: 20 }} fov={50}>
		<Threlte.OrbitControls autoRotate />
	</Threlte.PerspectiveCamera>

	<Threlte.AmbientLight intensity={0.2} />
	<Threlte.DirectionalLight
		shadow={{ camera: { top: 8 } }}
		intensity={2}
		position={{ x: 10, y: 20 }}
	/>

	<Threlte.Mesh
		geometry={new Three.SphereGeometry(4, 64, 64)}
		material={new Three.MeshStandardMaterial({ color: 'white' })}
		receiveShadow
		castShadow
		{...sphere}
	/>
	<Threlte.Mesh
		geometry={new Three.PlaneGeometry(20, 20)}
		material={new Three.MeshStandardMaterial({ color: 'white', side: Three.DoubleSide })}
		rotation={{ x: Utils.degToRad(-90) }}
		receiveShadow
	/>
</Threlte.Canvas>
