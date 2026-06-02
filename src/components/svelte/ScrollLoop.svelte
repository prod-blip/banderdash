<script>
  import { onMount } from 'svelte';

  let phase = $state('closed'); // closed | opening | open | scrolling | closing
  let label = $state('');
  let visible = $state(false);
  let containerEl = $state(null);
  let intervalId = null;

  const sequence = [
    { phase: 'opening', label: '', duration: 300 },
    { phase: 'open', label: 'open', duration: 800 },
    { phase: 'scrolling', label: 'scroll', duration: 1200 },
    { phase: 'closing', label: 'close', duration: 800 },
    { phase: 'closed', label: '', duration: 600 },
  ];

  function runLoop() {
    let step = 0;

    function next() {
      const current = sequence[step];
      phase = current.phase;
      label = current.label;

      step = (step + 1) % sequence.length;
      intervalId = setTimeout(next, current.duration);
    }

    next();
  }

  function stopLoop() {
    if (intervalId) {
      clearTimeout(intervalId);
      intervalId = null;
    }
    phase = 'closed';
    label = '';
  }

  onMount(() => {
    const observer = new IntersectionObserver(
      (entries) => {
        visible = entries[0].isIntersecting;
        if (visible) {
          runLoop();
        } else {
          stopLoop();
        }
      },
      { threshold: 0.3 }
    );

    if (containerEl) {
      observer.observe(containerEl);
    }

    return () => {
      observer.disconnect();
      stopLoop();
    };
  });
</script>

<div class="scroll-loop" bind:this={containerEl}>
  <div class="phone" class:screen-on={phase !== 'closed'}>
    <div class="phone__notch"></div>
    <div class="phone__screen">
      {#if phase === 'open' || phase === 'scrolling' || phase === 'closing'}
        <div class="feed" class:scrolling={phase === 'scrolling'}>
          <div class="feed__item"></div>
          <div class="feed__item"></div>
          <div class="feed__item"></div>
          <div class="feed__item"></div>
          <div class="feed__item"></div>
          <div class="feed__item"></div>
        </div>
      {/if}
    </div>
  </div>
  <div class="label" class:visible={label}>{label}</div>
</div>

<style>
  .scroll-loop {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1rem;
    margin-block: var(--space-xl, 2rem);
    padding: var(--space-lg, 1.5rem);
  }

  .phone {
    width: 80px;
    height: 160px;
    background: #1a1a1a;
    border-radius: 12px;
    padding: 8px 4px;
    position: relative;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
  }

  .phone__notch {
    width: 24px;
    height: 4px;
    background: #000;
    border-radius: 2px;
    margin: 0 auto 6px;
  }

  .phone__screen {
    width: 100%;
    height: calc(100% - 10px);
    background: #000;
    border-radius: 6px;
    overflow: hidden;
    transition: background 0.3s ease;
  }

  .phone.screen-on .phone__screen {
    background: #f5f5f5;
  }

  .feed {
    display: flex;
    flex-direction: column;
    gap: 6px;
    padding: 6px;
    transform: translateY(0);
    transition: transform 0.1s linear;
  }

  .feed.scrolling {
    animation: scroll-feed 1.2s ease-in-out;
  }

  .feed__item {
    height: 28px;
    background: linear-gradient(90deg, #e0e0e0 0%, #d0d0d0 50%, #e0e0e0 100%);
    border-radius: 4px;
  }

  @keyframes scroll-feed {
    0% {
      transform: translateY(0);
    }
    30% {
      transform: translateY(-40px);
    }
    60% {
      transform: translateY(-20px);
    }
    80% {
      transform: translateY(-60px);
    }
    100% {
      transform: translateY(-30px);
    }
  }

  .label {
    font-size: 0.85rem;
    font-style: italic;
    color: var(--color-text-muted, #6b6b6b);
    height: 1.2em;
    opacity: 0;
    transition: opacity 0.2s ease;
  }

  .label.visible {
    opacity: 1;
  }
</style>
