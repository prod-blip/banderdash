<script>
  let phase = $state('idle'); // idle | dissolving | arrested
  let containerEl = $state(null);

  function handleDelete() {
    if (phase !== 'idle') return;
    phase = 'dissolving';

    setTimeout(() => {
      phase = 'arrested';
    }, 2000);
  }

  function handleContinue() {
    phase = 'idle';
  }
</script>

<div class="delete-zone" class:dissolving={phase === 'dissolving'} class:arrested={phase === 'arrested'} bind:this={containerEl}>
  {#if phase === 'idle'}
    <button class="delete-btn" onclick={handleDelete}>
      Delete this article
    </button>
  {/if}

  {#if phase === 'dissolving'}
    <div class="dissolve-overlay">
      <div class="dissolve-text">
        <span class="char" style="--i:0">D</span><span class="char" style="--i:1">e</span><span class="char" style="--i:2">l</span><span class="char" style="--i:3">e</span><span class="char" style="--i:4">t</span><span class="char" style="--i:5">i</span><span class="char" style="--i:6">n</span><span class="char" style="--i:7">g</span><span class="char" style="--i:8">.</span><span class="char" style="--i:9">.</span><span class="char" style="--i:10">.</span>
      </div>
    </div>
  {/if}

  {#if phase === 'arrested'}
    <div class="arrest">
      <p class="arrest__line">I almost let you.</p>
      <p class="arrest__line arrest__line--delay">I almost let myself.</p>
      <button class="arrest__continue" onclick={handleContinue}>Keep reading</button>
    </div>
  {/if}
</div>

<style>
  .delete-zone {
    margin-block: var(--space-xl, 2rem);
    padding: var(--space-lg, 1.5rem);
    text-align: center;
    min-height: 80px;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
  }

  .delete-btn {
    background: none;
    border: 1px solid var(--color-border, #ddd);
    color: var(--color-text-muted, #6b6b6b);
    padding: 0.4rem 1rem;
    font-size: 0.8rem;
    font-family: system-ui, -apple-system, 'Segoe UI', sans-serif;
    cursor: pointer;
    border-radius: 3px;
    transition: all 0.3s ease;
    opacity: 0.6;
  }

  .delete-btn:hover {
    border-color: #c0392b;
    color: #c0392b;
    opacity: 1;
  }

  .dissolve-overlay {
    position: fixed;
    inset: 0;
    background: var(--color-bg, #faf9f6);
    z-index: 100;
    display: flex;
    align-items: center;
    justify-content: center;
    animation: fadeIn 0.3s ease;
  }

  .dissolve-text {
    font-family: system-ui, -apple-system, 'Segoe UI', sans-serif;
    font-size: 1.2rem;
    color: var(--color-text-muted, #6b6b6b);
  }

  .char {
    display: inline-block;
    animation: scatter 2s ease forwards;
    animation-delay: calc(var(--i) * 0.08s);
  }

  @keyframes scatter {
    0% {
      transform: translate(0, 0) rotate(0deg);
      opacity: 1;
    }
    60% {
      opacity: 1;
    }
    100% {
      transform: translate(
        calc((var(--i) - 5) * 30px),
        calc(sin(var(--i)) * 80px)
      ) rotate(calc(var(--i) * 25deg));
      opacity: 0;
    }
  }

  @keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
  }

  .arrest {
    position: fixed;
    inset: 0;
    background: var(--color-bg, #faf9f6);
    z-index: 100;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 0.75rem;
    animation: fadeIn 0.8s ease;
  }

  .arrest__line {
    font-style: italic;
    color: var(--color-text, #1a1a1a);
    font-size: 1.1rem;
    animation: fadeIn 0.8s ease both;
  }

  .arrest__line--delay {
    animation-delay: 0.6s;
  }

  .arrest__continue {
    margin-top: 2rem;
    background: none;
    border: 1px solid var(--color-border, #ddd);
    padding: 0.5rem 1.25rem;
    border-radius: 4px;
    cursor: pointer;
    color: var(--color-text-muted, #6b6b6b);
    font-size: 0.85rem;
    transition: all 0.2s ease;
    animation: fadeIn 1s ease both;
    animation-delay: 1.4s;
  }

  .arrest__continue:hover {
    border-color: var(--color-text, #1a1a1a);
    color: var(--color-text, #1a1a1a);
  }
</style>
