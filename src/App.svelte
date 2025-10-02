<script lang="ts">
  import { onMount } from 'svelte';
  
  let red = 128;
  let green = 128;
  let blue = 128;
  
  let currentMood = { name: 'Neutral', emoji: '😐', description: 'Balanced' };
  let quote = { text: 'Loading...', author: '' };
  let isLoadingQuote = false;
  
  let audioContext;
  
  onMount(() => {
    const AudioContextClass = window.AudioContext || (window as any).webkitAudioContext;
    audioContext = new AudioContextClass();
    fetchQuote();
  });
  
  $: {
    currentMood = calculateMood(red, green, blue);
  }
  
  $: backgroundColor = `rgb(${red}, ${green}, ${blue})`;
  
  $: brightness = (red * 0.299 + green * 0.587 + blue * 0.114);
  
  $: textColor = brightness > 128 ? '#000000' : '#FFFFFF';
  
  function calculateMood(r, g, b) {
    const total = r + g + b;
    const brightness = (r * 0.299 + g * 0.587 + b * 0.114);
    
    const redRatio = r / total;
    const greenRatio = g / total;
    const blueRatio = b / total;
    
    if (redRatio > 0.45 && brightness > 150) {
      return { name: 'Energetic', emoji: '🔥', description: 'Full of passion and energy' };
    } else if (redRatio > 0.4 && brightness < 100) {
      return { name: 'Intense', emoji: '💪', description: 'Strong and determined' };
    } else if (blueRatio > 0.45 && brightness < 100) {
      return { name: 'Mysterious', emoji: '🌙', description: 'Deep and enigmatic' };
    } else if (blueRatio > 0.4 && brightness > 150) {
      return { name: 'Calm', emoji: '🌊', description: 'Peaceful and serene' };
    } else if (greenRatio > 0.45 && brightness > 100) {
      return { name: 'Natural', emoji: '🌿', description: 'Fresh and renewing' };
    } else if (greenRatio > 0.4 && brightness < 100) {
      return { name: 'Earthy', emoji: '🌲', description: 'Connected and stable' };
    } else if (redRatio > 0.35 && greenRatio > 0.35 && brightness > 180) {
      return { name: 'Joyful', emoji: '☀️', description: 'Bright and optimistic' };
    } else if (redRatio > 0.35 && blueRatio > 0.35 && brightness > 150) {
      return { name: 'Creative', emoji: '🎨', description: 'Imaginative and artistic' };
    } else if (brightness < 50) {
      return { name: 'Deep', emoji: '🌑', description: 'Introspective and thoughtful' };
    } else if (brightness > 200) {
      return { name: 'Radiant', emoji: '✨', description: 'Luminous and pure' };
    } else {
      return { name: 'Neutral', emoji: '😐', description: 'Balanced and centered' };
    }
  }
  
  function playMoodSound(moodName) {
    if (!audioContext) return;
    
    const oscillator = audioContext.createOscillator();
    const gainNode = audioContext.createGain();
    
    oscillator.connect(gainNode);
    gainNode.connect(audioContext.destination);
    
    switch(moodName) {
      case 'Energetic':
        oscillator.frequency.value = 440;
        oscillator.type = 'square';
        break;
      case 'Intense':
        oscillator.frequency.value = 220;
        oscillator.type = 'sawtooth';
        break;
      case 'Mysterious':
        oscillator.frequency.value = 150;
        oscillator.type = 'triangle';
        break;
      case 'Calm':
        oscillator.frequency.value = 261.63;
        oscillator.type = 'sine';
        break;
      case 'Natural':
        oscillator.frequency.value = 329.63;
        oscillator.type = 'triangle';
        break;
      case 'Earthy':
        oscillator.frequency.value = 196;
        oscillator.type = 'triangle';
        break;
      case 'Joyful':
        oscillator.frequency.value = 523.25;
        oscillator.type = 'sine';
        break;
      case 'Creative':
        oscillator.frequency.value = 493.88;
        oscillator.type = 'triangle';
        break;
      case 'Deep':
        oscillator.frequency.value = 82.41;
        oscillator.type = 'sawtooth';
        break;
      case 'Radiant':
        oscillator.frequency.value = 659.25;
        oscillator.type = 'sine';
        break;
      default:
        oscillator.frequency.value = 440;
        oscillator.type = 'sine';
    }
    
    gainNode.gain.setValueAtTime(0.3, audioContext.currentTime);
    gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.5);
    
    oscillator.start(audioContext.currentTime);
    oscillator.stop(audioContext.currentTime + 0.5);
  }
  
  async function fetchQuote() {
    isLoadingQuote = true;
    
    const quotes = [
      { text: 'Sebas is going to buy a Jagermeister.', author: 'Anonymous' },
      { text: 'Mariana likes cocktails that cost 40 thousand pesos.', author: 'Anonymous' },
      { text: 'The Frencous never go to class.', author: 'Anonymous' },
      { text: 'Esteban is the class monitor.', author: 'Anonymous' },
      { text: 'Creativity is intelligence having fun.', author: 'Albert Einstein' },
      { text: 'Svelte is fire.', author: 'Anonymous' },
      { text: 'Art does not reproduce what is visible, it makes visible.', author: 'Paul Klee' }
    ];
    
    await new Promise(resolve => setTimeout(resolve, 300));
    
    const randomQuote = quotes[Math.floor(Math.random() * quotes.length)];
    quote = randomQuote;
    
    isLoadingQuote = false;
  }
  
  function handleMoodClick() {
    playMoodSound(currentMood.name);
    fetchQuote();
  }
</script>

<main style="background-color: {backgroundColor}; color: {textColor};">
  <div class="container">
    <h1>🎨 Mood Colors</h1>
    <p class="subtitle">Discover your mood through colors</p>
    
    <div class="mood-display" on:click={handleMoodClick} on:keypress={handleMoodClick} role="button" tabindex="0">
      <div class="emoji">{currentMood.emoji}</div>
      <h2>{currentMood.name}</h2>
      <p class="mood-description">{currentMood.description}</p>
      <span class="tap-hint">👆 Tap to hear</span>
    </div>
    
    <div class="sliders">
      <div class="slider-group">
        <label>
          <span class="slider-label">🔴 Red: {red}</span>
          <input 
            type="range" 
            min="0" 
            max="255" 
            bind:value={red}
            class="slider red-slider"
          />
        </label>
      </div>
      
      <div class="slider-group">
        <label>
          <span class="slider-label">🟢 Green: {green}</span>
          <input 
            type="range" 
            min="0" 
            max="255" 
            bind:value={green}
            class="slider green-slider"
          />
        </label>
      </div>
      
      <div class="slider-group">
        <label>
          <span class="slider-label">🔵 Blue: {blue}</span>
          <input 
            type="range" 
            min="0" 
            max="255" 
            bind:value={blue}
            class="slider blue-slider"
          />
        </label>
      </div>
    </div>
    
    <div class="quote-section">
      {#if isLoadingQuote}
        <p class="quote-loading">Loading inspiration...</p>
      {:else}
        <blockquote>
          <p class="quote-text">"{quote.text}"</p>
          <footer>— {quote.author}</footer>
        </blockquote>
      {/if}
      <button on:click={fetchQuote} class="refresh-btn">
        🔄 New Quote
      </button>
    </div>
    
    <div class="rgb-display">
      rgb({red}, {green}, {blue})
    </div>
  </div>
</main>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  }
  
  main {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 2rem;
    transition: background-color 0.3s ease, color 0.3s ease;
  }
  
  .container {
    max-width: 600px;
    width: 100%;
  }
  
  h1 {
    text-align: center;
    font-size: 3rem;
    margin: 0 0 0.5rem 0;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
  }
  
  .subtitle {
    text-align: center;
    font-size: 1.1rem;
    margin: 0 0 2rem 0;
    opacity: 0.9;
  }
  
  .mood-display {
    text-align: center;
    padding: 2rem;
    margin: 2rem 0;
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(10px);
    border-radius: 20px;
    cursor: pointer;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
  }
  
  .mood-display:hover {
    transform: scale(1.05);
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
  }
  
  .emoji {
    font-size: 5rem;
    margin: 0;
    animation: float 3s ease-in-out infinite;
  }
  
  @keyframes float {
    0%, 100% { transform: translateY(0px); }
    50% { transform: translateY(-10px); }
  }
  
  .mood-display h2 {
    font-size: 2rem;
    margin: 1rem 0 0.5rem 0;
  }
  
  .mood-description {
    font-size: 1.1rem;
    opacity: 0.9;
    margin: 0;
  }
  
  .tap-hint {
    display: block;
    margin-top: 1rem;
    font-size: 0.9rem;
    opacity: 0.7;
  }
  
  .sliders {
    margin: 2rem 0;
  }
  
  .slider-group {
    margin: 1.5rem 0;
  }
  
  .slider-label {
    display: block;
    font-size: 1.1rem;
    font-weight: 600;
    margin-bottom: 0.5rem;
  }
  
  .slider {
    width: 100%;
    height: 12px;
    border-radius: 6px;
    outline: none;
    appearance: none;
    -webkit-appearance: none;
    cursor: pointer;
  }
  
  .slider::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    width: 24px;
    height: 24px;
    border-radius: 50%;
    background: white;
    cursor: pointer;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
  }
  
  .slider::-moz-range-thumb {
    width: 24px;
    height: 24px;
    border-radius: 50%;
    background: white;
    cursor: pointer;
    border: none;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
  }
  
  .red-slider {
    background: linear-gradient(to right, #000000, #ff0000);
  }
  
  .green-slider {
    background: linear-gradient(to right, #000000, #00ff00);
  }
  
  .blue-slider {
    background: linear-gradient(to right, #000000, #0000ff);
  }
  
  .quote-section {
    margin: 2rem 0;
    padding: 2rem;
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(10px);
    border-radius: 20px;
  }
  
  blockquote {
    margin: 0;
    padding: 0;
  }
  
  .quote-text {
    font-size: 1.2rem;
    font-style: italic;
    line-height: 1.6;
    margin: 0 0 1rem 0;
  }
  
  blockquote footer {
    text-align: right;
    font-size: 1rem;
    opacity: 0.8;
  }
  
  .quote-loading {
    text-align: center;
    font-style: italic;
    opacity: 0.7;
  }
  
  .refresh-btn {
    display: block;
    margin: 1.5rem auto 0;
    padding: 0.75rem 1.5rem;
    background: rgba(255, 255, 255, 0.2);
    border: 2px solid currentColor;
    border-radius: 10px;
    color: inherit;
    font-size: 1rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.2s ease;
  }
  
  .refresh-btn:hover {
    background: rgba(255, 255, 255, 0.3);
    transform: scale(1.05);
  }
  
  .rgb-display {
    text-align: center;
    font-family: 'Courier New', monospace;
    font-size: 1.2rem;
    margin-top: 2rem;
    padding: 1rem;
    background: rgba(0, 0, 0, 0.2);
    border-radius: 10px;
    font-weight: 600;
  }
  
  @media (max-width: 600px) {
    h1 {
      font-size: 2rem;
    }
    
    .emoji {
      font-size: 4rem;
    }
    
    .mood-display h2 {
      font-size: 1.5rem;
    }
  }
</style>