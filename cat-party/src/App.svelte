<script>
  import { tick } from 'svelte';
  import { Confetti } from 'svelte-confetti'
  import { sound } from 'svelte-sound'
  import pew from './assets/pew.mp3'
  import img from './assets/cat.png'
  import corner from './assets/meow.png'
  
  let visible = false;
  let here = false;
  
  const spark = async () => {
    visible = false;
    await tick();
    visible = true;
  }

  const handleMouseEnter = () => (here = true);
  const handleMouseLeave = () => (here = false);
</script> 

<svelte:body on:mouseenter={handleMouseEnter} on:mouseleave={handleMouseLeave} />

<main>
  <div class="relative">
    <button use:sound={{ src: pew, events: ["click"] }} on:click={spark}>
      <img src={img} alt="cat" width="200px" />
    </button>
    {#if visible}
      <div class="confetti">
        <Confetti y={[-1, 1]} x={[-1, 1]} amount={100} />
      </div>
    {/if}
  </div>
  <img
    class="corner"
    class:curious={here}
    src={corner}
    alt="corner cat"
    width="400px"
  />
  {#if here}
      <div class="cornerConfetti">
        <Confetti y={[0, 3]} x={[0, 3]} amount={200} />
      </div>
  {/if}
</main>

<style>
  .relative {
    position: relative;
  }

  .relative .confetti {
    position: absolute;
    top: 50%;
    left: 50%;
  }

  .relative button {
      border: 0px;
      background-color: #fff;
  }

  .relative button:focus {
      outline: 0px;
  }

  .confetti {
    pointer-events: none;
  }

  .cornerConfetti {
    position: absolute;
		left: 0px;
		bottom: 0px;
  }

  .corner {
		position: absolute;
		left: -40px;
		bottom: 0px;
		transform: translate(-80%, 0) rotate(-30deg);
		transform-origin: 100% 100%;
		transition: transform 0.4s;
    z-index: 1000;
	}

	.curious {
		transform: translate(-15%, 0) rotate(0deg);
	}

	:global(body) {
		overflow: hidden;
	}
</style>
