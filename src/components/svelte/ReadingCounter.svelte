<script>
  import { onMount } from 'svelte';

  let elapsed = $state(0);
  let revealed = $state(false);

  onMount(() => {
    const interval = setInterval(() => {
      elapsed += 1;
    }, 1000);

    function onReveal() {
      revealed = true;
    }

    window.addEventListener('reveal-counter', onReveal);

    return () => {
      clearInterval(interval);
      window.removeEventListener('reveal-counter', onReveal);
    };
  });

  function formatTime(seconds) {
    const m = Math.floor(seconds / 60);
    const s = seconds % 60;
    if (m === 0) return `${s}s`;
    return `${m}m ${s}s`;
  }
</script>

<div class="counter" class:revealed>
  <span class="counter__dot"></span>
  {#if revealed}
    <span class="counter__label">You've been here {formatTime(elapsed)}. You're still here.</span>
  {:else}
    <span class="counter__time">{formatTime(elapsed)}</span>
  {/if}
</div>

<style>
  .counter {
    position: fixed;
    bottom: var(--space-lg, 1.5rem);
    right: var(--space-lg, 1.5rem);
    font-family: system-ui, -apple-system, 'Segoe UI', sans-serif;
    font-size: 0.7rem;
    color: var(--color-text-muted, #999);
    display: flex;
    align-items: center;
    gap: 0.4rem;
    opacity: 0.3;
    transition: all 0.6s ease;
    z-index: 50;
    pointer-events: none;
  }

  .counter.revealed {
    opacity: 1;
    font-size: 0.85rem;
    color: var(--color-text, #1a1a1a);
    pointer-events: auto;
  }

  .counter__dot {
    width: 6px;
    height: 6px;
    border-radius: 50%;
    background: var(--color-accent, #c44d2b);
    animation: pulse 2s ease-in-out infinite;
  }

  .counter.revealed .counter__dot {
    animation: none;
    background: var(--color-accent, #c44d2b);
  }

  .counter__time {
    font-variant-numeric: tabular-nums;
  }

  .counter__label {
    font-style: italic;
  }

  @keyframes pulse {
    0%, 100% { opacity: 0.3; }
    50% { opacity: 1; }
  }

  @media (max-width: 600px) {
    .counter {
      bottom: var(--space-sm, 0.75rem);
      right: var(--space-sm, 0.75rem);
      left: var(--space-sm, 0.75rem);
      justify-content: center;
      font-size: 0.65rem;
      background: var(--color-bg, #faf9f6);
      padding: 0.4rem 0.6rem;
      border-radius: 4px;
    }

    .counter.revealed {
      font-size: 0.75rem;
    }
  }
</style>
