<script>
  import { Canvas } from '@threlte/core';
  import Carousel from '$components/Carousel.svelte';
  import Scene from '$components/Scene.svelte';

  /** @type {import('./$types').PageData} */
  export let data;

  $: clothesCollection = data.products[1].node.products.edges;
  $: featuredCollection = data.products[1].node.products.edges;
</script>

<svelte:head>
  <title>Home – SvelteKit Commerce</title>
</svelte:head>

<main>
  <section class="canvas">
      <Canvas>
        <Scene /> 
      </Canvas>
  </section>
  <section>
    <Carousel items={clothesCollection} />
  </section>
  <section>
    <div
      class="flex flex-col px-8 py-20 border border-black bg-dark lg:flex-row lg:items-center"
    >
      <div
        class="flex-none mb-4 mr-8 text-3xl font-black text-left md:text-4xl lg:mb-0 lg:w-1/3 lg:text-right lg:text-6xl"
      >
        Dessert dragée halvah croissant.
      </div>
      <div>
        <div class="lg:text-2xl">
          Cupcake ipsum dolor sit amet lemon drops pastry cotton candy. Sweet carrot cake macaroon
          bonbon croissant fruitcake jujubes macaroon oat cake. Soufflé bonbon caramels jelly beans.
          Tiramisu sweet roll cheesecake pie carrot cake.
        </div>
        <button class="mt-4 font-bold text-svelteOrange hover:text-svelteDark lg:text-2xl">
          Read it here
        </button>
      </div>
    </div>
  </section>
</main>

<style>
  .canvas {
    height: 80vh;
  }
</style>
