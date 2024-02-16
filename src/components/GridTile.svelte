<script>
  export let title = '';
  export let removeLabels = false;
  export let imageSrc;
  export let price = '';
  export let currencyCode = '';
  export let href = '';
  export let priority = 'lazy';

  let hover = false;
</script>

<a
  data-test="grid-tile"
  {href}
  data-sveltekit-prefetch
>
  <img
    alt={title}
    class={`w-full md:w-1/2 lg:w-full transition duration-300 ease-in-out ${
      hover ? 'scale-110' : ''
    }`}
    fetchpriority={priority === 'eager' ? 'high' : 'low'}
    decoding="async"
    loading={priority}
    src={imageSrc || ''}
  />
</a>

{#if !removeLabels}
  <div>
    <a {href} class="tileHeader">
      {title}
    </a>
    <div>
      ${price}
      {currencyCode}
    </div>
  </div>
{/if}

<style>
.tileHeader {
 font-weight: bold; 
}
</style>
