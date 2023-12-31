<script lang="ts">
  import { T, useTask, useThrelte } from '@threlte/core';
  import { OrbitControls } from '@threlte/extras';
  import { Color, PlaneGeometry} from 'three';
  import { FontLoader } from 'three/addons/loaders/FontLoader.js';
  import { TextGeometry } from 'three/addons/geometries/TextGeometry.js';
  import { noise3 } from '$utils/perlin.js'; 

  const loader = new FontLoader();
  let textGeometry;
  const font = loader.load('/helvetiker.typeface.json', (font) => {
    textGeometry = new TextGeometry(
      'perlin noise',
      {
        size: 1,
        font: font,
        height: 0.2,
        curveSegments: 2,
        depth: 0.2
      }
    )
    textGeometry.computeBoundingBox();
  });
  let geometry = new PlaneGeometry(20, 20, 50, 50);
  let vertices = geometry.getAttribute('position').array;
  let maxFrames = 1000;
  let t = 0;

  useTask(() => {
    geometry = new PlaneGeometry(20, 20, 50, 50);
    vertices = geometry.getAttribute('position').array;
    for (let i = 0; i < vertices.length; i+=3) {
      const x = vertices[i];
      const y = vertices[i + 1];
      vertices[i + 2] = noise3(x / 3, y / 3, Math.sin(Math.PI * (t/500)))*5;
    }
    if (t < maxFrames) {
      t++;
    } else {
      t = 0;
    }
  });
</script>

<T.PerspectiveCamera
  makeDefault
  on:create={({ ref }) => {
    ref.position.set(20, 10, 30);
  }}
>
  <OrbitControls
    enableDamping
    autoRotate
    autoRotateSpeed={0.1}
    rotateSpeed={0.1}
    enablePan={false}
    target.y={0}
  />
</T.PerspectiveCamera>

<T.Mesh bind:geometry={geometry} rotation={[-Math.PI / 2, 0, 0]}>
  <T.MeshBasicMaterial
    wireframe={true}
    color={'lightgrey'}
  />
</T.Mesh>

{#if textGeometry}
<T.Mesh
  bind:geometry={textGeometry}
  position={[-10, 0, 10]}
  >
  <T.MeshBasicMaterial
    wireframe={true}
    color={"hotpink"}
  />
</T.Mesh>
{/if}
