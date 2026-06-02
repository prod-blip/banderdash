<script>
  import { onMount } from 'svelte';

  let visible = $state(false);
  let clicked = $state(false);
  let returning = $state(false);
  let position = $state({ top: 0 });

  onMount(() => {
    const timer = setTimeout(() => {
      position.top = Math.random() * 40 + 20;
      visible = true;
    }, 3000);

    return () => clearTimeout(timer);
  });

  function handleClick() {
    clicked = true;
  }

  function handleReturn() {
    returning = true;
    setTimeout(() => {
      clicked = false;
      returning = false;
      visible = false;
    }, 600);
  }
</script>

{#if visible && !clicked}
  <div class="drift" style="top: {position.top}%">
    <button class="drift__lure" onclick={handleClick}>
      <span class="drift__shimmer"></span>
      <span class="drift__text">Something else →</span>
    </button>
  </div>
{/if}

{#if clicked}
  <div class="tangent" class:returning>
    <div class="tangent__content">
      <p>You clicked it.</p>
      <p>You left the essay. Mid-sentence, probably. There was a shiny thing at the edge and you went for it.</p>
      <p>There's nothing here. There was never going to be anything here.</p>
      <p class="tangent__quiet">See?</p>
      <button class="tangent__back" onclick={handleReturn}>Go back to reading</button>
    </div>
  </div>
{/if}

<style>
  .drift {
    position: fixed;
    right: -0.5rem;
    z-index: 40;
    animation: slideIn 0.8s ease forwards;
  }

  @keyframes slideIn {
    from { transform: translateX(100%); opacity: 0; }
    to { transform: translateX(0); opacity: 1; }
  }

  .drift__lure {
    background: none;
    border: none;
    cursor: pointer;
    padding: 0.75rem 1rem;
    position: relative;
    color: var(--color-accent, #c44d2b);
    font-family: system-ui, -apple-system, 'Segoe UI', sans-serif;
    font-size: 0.8rem;
    letter-spacing: 0.02em;
  }

  .drift__shimmer {
    position: absolute;
    inset: 0;
    background: linear-gradient(
      90deg,
      transparent,
      color-mix(in srgb, var(--color-accent, #c44d2b) 10%, transparent),
      transparent
    );
    animation: shimmer 2s ease-in-out infinite;
    border-radius: 4px;
  }

  @keyframes shimmer {
    0%, 100% { opacity: 0; }
    50% { opacity: 1; }
  }

  .drift__text {
    position: relative;
  }

  .drift__lure:hover .drift__text {
    text-decoration: underline;
  }

  .tangent {
    position: fixed;
    inset: 0;
    z-index: 100;
    background: var(--color-bg, #faf9f6);
    display: flex;
    align-items: center;
    justify-content: center;
    animation: fadeIn 0.4s ease;
  }

  .tangent.returning {
    animation: fadeOut 0.6s ease forwards;
  }

  @keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
  }

  @keyframes fadeOut {
    from { opacity: 1; }
    to { opacity: 0; }
  }

  .tangent__content {
    max-width: 40ch;
    text-align: center;
    padding: var(--space-xl, 2rem);
  }

  .tangent__content p {
    margin-bottom: 1rem;
    color: var(--color-text-muted, #6b6b6b);
    line-height: 1.6;
  }

  .tangent__quiet {
    margin-top: 1.5rem;
    font-style: italic;
    color: var(--color-text, #1a1a1a);
    font-size: 1.1rem;
  }

  .tangent__back {
    margin-top: 2rem;
    background: none;
    border: 1px solid var(--color-border, #ddd);
    padding: 0.5rem 1.25rem;
    border-radius: 4px;
    cursor: pointer;
    color: var(--color-text-muted, #6b6b6b);
    font-size: 0.85rem;
    transition: all 0.2s ease;
  }

  .tangent__back:hover {
    border-color: var(--color-text, #1a1a1a);
    color: var(--color-text, #1a1a1a);
  }
</style>
