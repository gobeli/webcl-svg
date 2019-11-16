<script>
  import Eyes from './Eyes.svelte';

  let activeSlide = 1;

  function onKeyDown(e) {
    switch (e.keyCode ) {
      case 39: // right arrow
        activeSlide++;
        break;
      case 37: // left arrow
        activeSlide--;
        break;
    }
  }

</script>

<style>
	.slide {
		border: 1px solid #eee;
		display: flex;
		flex-direction: column;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .slides {
    background-color: #333;
    width: 100%;
    height: 100%;
    overflow: hidden;
    position: relative;
  }
  h1 {
    color: #eee;
    font-size: 3.5rem;
  }
  h1 a {
    color: inherit;
    text-decoration: underline;
  }
</style>

<svelte:window on:keydown={onKeyDown}/>

<div class="slides">
  {#if activeSlide === 1}
    <div class="slide">
      <h1>Svelte</h1>
    </div>
  {/if}
  {#if activeSlide === 2}
    <div class="slide">
      <h1>Simple <a href="https://svelte.dev/repl/hello-world?version=3.14.1" target="_blank">example</a></h1>
    </div>
  {/if}
  {#if activeSlide === 3}
    <div class="slide">
      <Eyes />
    </div>
  {/if}
</div>


