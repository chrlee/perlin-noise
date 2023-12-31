<script lang="ts">
  import { T, useTask } from '@threlte/core';
  import { OrbitControls } from '@threlte/extras';
  import { PlaneGeometry} from 'three';
  import { FontLoader } from 'three/addons/loaders/FontLoader.js';
  import { TextGeometry } from 'three/addons/geometries/TextGeometry.js';
  import { noise3 } from '$utils/perlin.js'; 

  const loader = new FontLoader();
  let textGeometry;
  loader.load('/helvetiker.typeface.json', (font) => {
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
  let vertices = geometry.getAttribute('position');
  let maxFrames = 1000;
  let t = 0;

  useTask(() => {
    for (let i = 0; i < vertices.array.length; i+=3) {
      const x = vertices.array[i];
      const y = vertices.array[i + 1];
      vertices.array[i + 2] = noise3(x / 3, y / 3, Math.sin(Math.PI * (t/500)))*5;
    }
    vertices.needsUpdate = true;
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
    switch (screen.orientation.type) {
      case 'portrait-primary':
      case 'portrait-secondary':
        ref.position.set(40, 20, 60);
        break;
      default:
        ref.position.set(15, 15, 30);
        break;
    }
  }}
>
  <OrbitControls
    enableDamping
    autoRotate
    autoRotateSpeed={0.15}
    rotateSpeed={0.15}
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
