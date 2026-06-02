<script>
  let expandedIndex = $state(-1);

  const fragments = [
    {
      title: 'On Building Things Nobody Asked For',
      snippet: 'There is a particular kind of vanity in making something nobody asked for. You convince yourself the world needs it. You sketch architectures on napkins. You tell people about it at parties and they nod politely. Three weeks later the repo has four commits and a README that promises',
    },
    {
      title: 'Why I Quit (Draft 3)',
      snippet: 'The third time you write a resignation letter it stops feeling brave. The first time, your hands shook. The second time, you edited it for an hour. The third time, you saved it to Desktop and opened Twitter instead. There is a version of me who sent it. That version lives in a',
    },
    {
      title: 'A Taxonomy of Distractions',
      snippet: 'Category one: the phone. Not even a notification — the possibility of a notification. The phantom buzz. Category two: the adjacent tab. You opened it twenty minutes ago to look up a function signature and now you are reading about the history of',
    },
    {
      title: 'Letter to Myself at 22',
      snippet: "You\u2019re going to think that speed is the important thing. That the people who ship fast win. This is true, sort of, but it misses something: the ones who ship fast aren\u2019t faster than you. They just don\u2019t",
    },
    {
      title: 'The Middle of the Book',
      snippet: 'Nobody talks about the middle of the book. They talk about starting — that rush of the blank page, the potential, all that white space. And they talk about finishing — the satisfaction, the weight of it in your hands. But the middle. The middle is where you realize the book you are writing is not the book you thought you were writing, and also you are',
    },
    {
      title: 'Untitled (Notes)',
      snippet: "Things I want to say but haven\u2019t figured out how: that trying is embarrassing, that most ambition is just fear wearing a blazer, that I have started this paragraph four times and",
    },
  ];

  function toggle(index) {
    expandedIndex = expandedIndex === index ? -1 : index;
  }
</script>

<div class="graveyard">
  <div class="graveyard__label">Abandoned drafts — you can't finish any of them either</div>
  <div class="graveyard__pile">
    {#each fragments as fragment, i}
      <button
        class="fragment"
        class:expanded={expandedIndex === i}
        onclick={() => toggle(i)}
        style="--offset: {(i % 3 - 1) * 4}px; --rotate: {(i % 5 - 2) * 1.5}deg"
      >
        <span class="fragment__title">{fragment.title}</span>
        {#if expandedIndex === i}
          <p class="fragment__text">{fragment.snippet}—</p>
          <span class="fragment__end">[ that's all there is ]</span>
        {/if}
      </button>
    {/each}
  </div>
</div>

<style>
  .graveyard {
    margin-block: var(--space-2xl, 3rem);
  }

  .graveyard__label {
    font-family: system-ui, -apple-system, 'Segoe UI', sans-serif;
    font-size: var(--text-sm, 0.8rem);
    color: var(--color-text-muted, #6b6b6b);
    margin-bottom: var(--space-lg, 1.5rem);
    text-align: center;
    font-style: italic;
  }

  .graveyard__pile {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
    max-width: 50ch;
    margin-inline: auto;
  }

  .fragment {
    background: var(--color-surface, #fff);
    border: 1px solid var(--color-border, #ddd);
    border-radius: 3px;
    padding: 0.75rem 1rem;
    text-align: left;
    cursor: pointer;
    transform: translateX(var(--offset)) rotate(var(--rotate));
    transition: all 0.3s ease;
    font-family: inherit;
    font-size: inherit;
    color: inherit;
    line-height: 1.6;
  }

  .fragment:hover {
    transform: translateX(0) rotate(0deg);
    border-color: var(--color-accent-muted, #d4836e);
  }

  .fragment.expanded {
    transform: translateX(0) rotate(0deg);
    border-color: var(--color-accent-muted, #d4836e);
  }

  .fragment__title {
    font-family: system-ui, -apple-system, 'Segoe UI', sans-serif;
    font-weight: 600;
    font-size: 0.9rem;
    display: block;
  }

  .fragment__text {
    margin-top: 0.5rem;
    color: var(--color-text-muted, #6b6b6b);
    font-size: 0.9rem;
  }

  .fragment__end {
    display: block;
    margin-top: 0.75rem;
    font-size: 0.75rem;
    color: var(--color-text-muted, #6b6b6b);
    font-style: italic;
    text-align: center;
  }
</style>
