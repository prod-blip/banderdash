<script>
  import { onMount } from 'svelte';

  let looped = $state(false);
  let sentinel = $state(null);

  onMount(() => {
    if (!sentinel) return;

    const observer = new IntersectionObserver(
      (entries) => {
        for (const entry of entries) {
          if (entry.isIntersecting && !looped) {
            looped = true;
            // Find the essay's opening area and scroll to it
            const target = document.querySelector('[data-loop-target]');
            if (target) {
              // Small delay so reader sees the trigger
              setTimeout(() => {
                target.scrollIntoView({ behavior: 'smooth' });
              }, 400);
            }
          }
        }
      },
      { threshold: 0.5 }
    );

    observer.observe(sentinel);
    return () => observer.disconnect();
  });
</script>

<div bind:this={sentinel} class="loop-sentinel">
  {#if looped}
    <p class="loop-text" data-loop-mark>And the loop goes on.</p>
  {/if}
</div>

<style>
  .loop-sentinel {
    min-height: 1px;
    margin-block: var(--space-xl, 2rem);
  }

  .loop-text {
    text-align: center;
    font-style: italic;
    color: var(--color-text-muted, #6b6b6b);
    font-size: var(--text-sm, 0.8rem);
    animation: fadeIn 0.8s ease;
    padding: var(--space-lg, 1.5rem);
  }

  @keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
  }
</style>
