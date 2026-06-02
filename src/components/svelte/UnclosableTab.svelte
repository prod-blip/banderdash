<script>
  let visible = $state(true);
  let closing = $state(false);

  function handleClose() {
    if (closing) return;
    closing = true;
    visible = false;

    setTimeout(() => {
      visible = true;
      closing = false;
    }, 300);
  }
</script>

<span class="tab" class:hidden={!visible}>
  <span class="tab__text">forty-three browser tabs</span>
  <button class="tab__close" onclick={handleClose} aria-label="Close tab">
    <svg width="10" height="10" viewBox="0 0 10 10" fill="none" stroke="currentColor" stroke-width="1.5">
      <path d="M1 1l8 8M9 1l-8 8" />
    </svg>
  </button>
</span>

<style>
  .tab {
    display: inline-flex;
    align-items: center;
    gap: 0.35em;
    background: var(--color-bg-elevated, #f5f4f1);
    border: 1px solid var(--color-border, #ddd);
    border-bottom: none;
    border-radius: 6px 6px 0 0;
    padding: 0.15em 0.4em 0.2em 0.5em;
    font-size: 0.95em;
    position: relative;
    top: 0.1em;
    transition: opacity 0.15s ease, transform 0.15s ease;
  }

  .tab.hidden {
    opacity: 0;
    transform: scale(0.95);
  }

  .tab__text {
    line-height: 1.3;
  }

  .tab__close {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    background: none;
    border: none;
    padding: 0.15em;
    cursor: pointer;
    color: var(--color-text-muted, #999);
    border-radius: 3px;
    transition: background 0.15s ease, color 0.15s ease;
    line-height: 1;
  }

  .tab__close:hover {
    background: var(--color-border, #ddd);
    color: var(--color-text, #1a1a1a);
  }

  .tab__close svg {
    display: block;
  }
</style>
