<script>
  import { onMount } from 'svelte';

  const scentText = "more like a scent and you follow it";
  const scentWords = scentText.split(' ');

  let containerEl = $state(null);
  let visible = $state(false);
  let trails = $state([]);
  let trailId = 0;

  onMount(() => {
    const observer = new IntersectionObserver(
      (entries) => {
        visible = entries[0].isIntersecting;
      },
      { threshold: 0.5 }
    );

    if (containerEl) {
      observer.observe(containerEl);
    }

    return () => observer.disconnect();
  });

  function handleMouseMove(e) {
    if (!containerEl) return;
    const rect = containerEl.getBoundingClientRect();
    const x = e.clientX - rect.left;
    const y = e.clientY - rect.top;

    const newTrail = {
      id: trailId++,
      x,
      y,
    };

    trails = [...trails.slice(-12), newTrail];

    setTimeout(() => {
      trails = trails.filter((t) => t.id !== newTrail.id);
    }, 600);
  }

  function handleMouseLeave() {
    trails = [];
  }
</script>

<span
  class="scent-container"
  bind:this={containerEl}
  onmousemove={handleMouseMove}
  onmouseleave={handleMouseLeave}
  role="text"
>
  <span class="static">then a thought appears, which is not even a thought, </span><span class="scent-part" class:visible>
    {#each scentWords as word, i}
      <span class="scent-word" style="--i:{i};">{word}</span>{' '}
    {/each}
  </span>

  {#each trails as trail (trail.id)}
    <span
      class="trail-dot"
      style="left:{trail.x}px; top:{trail.y}px;"
    ></span>
  {/each}
</span>

<style>
  .scent-container {
    position: relative;
    display: inline;
  }

  .static {
    display: inline;
  }

  .scent-part {
    display: inline;
  }

  .scent-word {
    display: inline-block;
    opacity: 0;
    filter: blur(8px);
    transform: translateY(4px);
    transition: opacity 0.6s ease, filter 0.8s ease, transform 0.6s ease;
    transition-delay: calc(var(--i) * 0.12s);
  }

  .scent-part.visible .scent-word {
    opacity: 1;
    filter: blur(0);
    transform: translateY(0);
  }

  .trail-dot {
    position: absolute;
    width: 6px;
    height: 6px;
    background: radial-gradient(circle, var(--color-text-muted, #999) 0%, transparent 70%);
    border-radius: 50%;
    pointer-events: none;
    opacity: 0.5;
    transform: translate(-50%, -50%);
    animation: fade-trail 0.6s ease-out forwards;
  }

  @keyframes fade-trail {
    0% {
      opacity: 0.5;
      transform: translate(-50%, -50%) scale(1);
    }
    100% {
      opacity: 0;
      transform: translate(-50%, -50%) scale(0.3);
    }
  }
</style>
