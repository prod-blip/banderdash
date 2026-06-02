<script>
  import { onMount } from 'svelte';

  let sentinel = $state(null);

  onMount(() => {
    if (!sentinel) return;

    const observer = new IntersectionObserver(
      (entries) => {
        for (const entry of entries) {
          if (entry.isIntersecting) {
            window.dispatchEvent(new CustomEvent('reveal-counter'));
            observer.disconnect();
          }
        }
      },
      { threshold: 0.5 }
    );

    observer.observe(sentinel);
    return () => observer.disconnect();
  });
</script>

<span bind:this={sentinel} class="reveal-sentinel"></span>

<style>
  .reveal-sentinel {
    display: block;
    height: 1px;
  }
</style>
