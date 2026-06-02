<script>
  import { onMount } from 'svelte';

  const steps = [
    "feel guilty about not finishing",
    "start something new to prove you can",
    "get excited",
    "hit the specific part",
    "feel the heat die",
    "abandon"
  ];

  let visible = $state(false);
  let containerEl = $state(null);

  onMount(() => {
    const observer = new IntersectionObserver(
      (entries) => {
        visible = entries[0].isIntersecting;
      },
      { threshold: 0.3 }
    );

    if (containerEl) {
      observer.observe(containerEl);
    }

    return () => observer.disconnect();
  });
</script>

<div class="guilt-loop" bind:this={containerEl}>
  <div class="orbit" class:spinning={visible}>
    {#each steps as step, i}
      <span
        class="step"
        style="--i:{i}; --total:{steps.length};"
      >
        {step}
      </span>
    {/each}
  </div>
  <div class="center-dot"></div>
</div>

<style>
  .guilt-loop {
    position: relative;
    width: 100%;
    height: 320px;
    margin-block: var(--space-xl, 2rem);
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
  }

  .orbit {
    position: relative;
    width: 260px;
    height: 260px;
    border-radius: 50%;
    border: 1px dashed var(--color-border, #ddd);
  }

  .orbit.spinning {
    animation: rotate 20s linear infinite;
  }

  .step {
    position: absolute;
    left: 50%;
    top: 50%;
    width: max-content;
    max-width: 120px;
    font-size: 0.75rem;
    color: var(--color-text-muted, #666);
    text-align: center;
    transform:
      rotate(calc(360deg / var(--total) * var(--i)))
      translateY(-130px)
      rotate(calc(-360deg / var(--total) * var(--i)));
    transform-origin: center center;
  }

  .orbit.spinning .step {
    animation: counter-rotate 20s linear infinite;
    animation-delay: calc(var(--i) * -0.1s);
  }

  .center-dot {
    position: absolute;
    width: 8px;
    height: 8px;
    background: var(--color-text-muted, #999);
    border-radius: 50%;
  }

  @keyframes rotate {
    from {
      transform: rotate(0deg);
    }
    to {
      transform: rotate(360deg);
    }
  }

  @keyframes counter-rotate {
    from {
      transform:
        rotate(calc(360deg / var(--total) * var(--i)))
        translateY(-130px)
        rotate(calc(-360deg / var(--total) * var(--i) - 0deg));
    }
    to {
      transform:
        rotate(calc(360deg / var(--total) * var(--i)))
        translateY(-130px)
        rotate(calc(-360deg / var(--total) * var(--i) - 360deg));
    }
  }
</style>
