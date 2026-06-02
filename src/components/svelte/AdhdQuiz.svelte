<script>
  import { onMount } from 'svelte';

  let currentQuestion = $state(0);
  let containerEl = $state(null);
  let started = $state(false);

  const questions = [
    {
      q: "How often do you find yourself starting tasks but not finishing them?",
      options: ["Rarely", "Sometimes", "Often", "I'm not sure I've ever finished anything"]
    },
    {
      q: "When reading, do you ever realize you've read the same paragraph three times?",
      options: ["Never", "Occasionally", "Frequently", "Wait, what was the question?"]
    },
    {
      q: "How many browser tabs do you have open right now?",
      options: ["Under 5", "5-15", "15-30", "I stopped counting"]
    },
    {
      q: null,
      reveal: true
    }
  ];

  function selectAnswer() {
    if (currentQuestion < questions.length - 1) {
      currentQuestion++;
    }
  }

  function startQuiz() {
    started = true;
    currentQuestion = 0;
  }

  function resetQuiz() {
    started = false;
    currentQuestion = 0;
  }
</script>

<div class="quiz" bind:this={containerEl}>
  {#if !started}
    <button class="quiz__start" onclick={startQuiz}>
      Take a quick ADHD quiz?
    </button>
  {:else if questions[currentQuestion].reveal}
    <div class="quiz__reveal">
      <p class="reveal__text">You just stopped reading an essay to take a quiz.</p>
      <p class="reveal__text reveal__text--delay">In the middle of a paragraph.</p>
      <p class="reveal__text reveal__text--delay2">About not finishing things.</p>
      <p class="reveal__subtext">You don't need a diagnosis. You have your answer.</p>
      <button class="quiz__back" onclick={resetQuiz}>Back to the essay</button>
    </div>
  {:else}
    <div class="quiz__question">
      <p class="question__number">Question {currentQuestion + 1} of 4</p>
      <p class="question__text">{questions[currentQuestion].q}</p>
      <div class="question__options">
        {#each questions[currentQuestion].options as option}
          <button class="option" onclick={selectAnswer}>{option}</button>
        {/each}
      </div>
    </div>
  {/if}
</div>

<style>
  .quiz {
    margin-block: var(--space-xl, 2rem);
    padding: var(--space-lg, 1.5rem);
    border: 1px solid var(--color-border, #e0e0e0);
    border-radius: 8px;
    background: var(--color-bg-elevated, #faf9f6);
  }

  .quiz__start {
    display: block;
    width: 100%;
    padding: 0.75rem 1rem;
    background: none;
    border: 1px dashed var(--color-border, #ccc);
    border-radius: 6px;
    color: var(--color-text-muted, #666);
    font-size: 0.9rem;
    cursor: pointer;
    transition: all 0.2s ease;
  }

  .quiz__start:hover {
    border-style: solid;
    color: var(--color-text, #1a1a1a);
    border-color: var(--color-text-muted, #999);
  }

  .quiz__question {
    animation: fadeIn 0.3s ease;
  }

  .question__number {
    font-size: 0.75rem;
    color: var(--color-text-muted, #999);
    margin-bottom: 0.5rem;
    text-transform: uppercase;
    letter-spacing: 0.05em;
  }

  .question__text {
    font-size: 1rem;
    margin-bottom: 1rem;
    color: var(--color-text, #1a1a1a);
  }

  .question__options {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  .option {
    padding: 0.6rem 1rem;
    background: var(--color-bg, #fff);
    border: 1px solid var(--color-border, #ddd);
    border-radius: 6px;
    text-align: left;
    font-size: 0.9rem;
    cursor: pointer;
    transition: all 0.15s ease;
    color: var(--color-text, #1a1a1a);
  }

  .option:hover {
    background: var(--color-bg-elevated, #f5f5f5);
    border-color: var(--color-text-muted, #999);
  }

  .quiz__reveal {
    text-align: center;
    padding: 1rem 0;
  }

  .reveal__text {
    font-size: 1rem;
    margin-bottom: 0.5rem;
    color: var(--color-text, #1a1a1a);
    animation: fadeIn 0.6s ease both;
  }

  .reveal__text--delay {
    animation-delay: 0.8s;
  }

  .reveal__text--delay2 {
    animation-delay: 1.6s;
  }

  .reveal__subtext {
    font-size: 0.85rem;
    font-style: italic;
    color: var(--color-text-muted, #666);
    margin-top: 1.5rem;
    animation: fadeIn 0.6s ease both;
    animation-delay: 2.4s;
  }

  .quiz__back {
    margin-top: 1.5rem;
    padding: 0.5rem 1rem;
    background: none;
    border: 1px solid var(--color-border, #ddd);
    border-radius: 4px;
    font-size: 0.85rem;
    color: var(--color-text-muted, #666);
    cursor: pointer;
    transition: all 0.2s ease;
    animation: fadeIn 0.6s ease both;
    animation-delay: 3s;
  }

  .quiz__back:hover {
    border-color: var(--color-text, #1a1a1a);
    color: var(--color-text, #1a1a1a);
  }

  @keyframes fadeIn {
    from {
      opacity: 0;
      transform: translateY(4px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
</style>
