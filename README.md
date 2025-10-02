# 🎨 Mood Colors

An interactive application built with **Svelte** that lets you explore different moods through RGB colors.

## ✨ Features

- 🎨 **Interactive RGB Sliders**: Control red, green, and blue levels
- 😊 **Mood Detection**: The system automatically identifies your mood based on the color
- 🔊 **Unique Sounds**: Each mood has its own characteristic sound
- 💬 **Inspirational Quotes**: 15 quotes about colors and art
- 🌈 **Real-time Color Changes**: Background updates instantly

## 🚀 How to Run

1. Clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/mood-colors.git
cd mood-colors
```

2. Install dependencies:

```bash
npm install
```

3. Run the development server:

```bash
npm run dev
```

4. Open your browser at `http://localhost:5173/`

## 🛠️ Technologies

- **Svelte** - Reactive framework
- **TypeScript** - Static typing
- **Vite** - Build tool
- **Web Audio API** - Sound generation

## 📚 Svelte Concepts Demonstrated

- ✅ Reactivity with `$:`
- ✅ Two-way binding with `bind:value`
- ✅ Event handlers (`on:click`, `on:keypress`)
- ✅ Conditional rendering (`{#if}...{:else}...{/if}`)
- ✅ Lifecycle hooks (`onMount`)
- ✅ Scoped styles

## 🎨 Available Moods

- 🔥 **Energetic** - Bright red colors
- 💪 **Intense** - Dark reds
- 🌙 **Mysterious** - Deep blues
- 🌊 **Calm** - Light blues
- 🌿 **Natural** - Bright greens
- 🌲 **Earthy** - Dark greens
- ☀️ **Joyful** - Bright yellows
- 🎨 **Creative** - Purples
- 🌑 **Deep** - Very dark colors
- ✨ **Radiant** - Very bright colors

## 🎯 Learning Purpose

This project demonstrates core Svelte concepts in a practical, interactive way:

- **Reactivity**: How Svelte automatically updates the UI when data changes
- **Component Structure**: Single-file components with script, markup, and styles
- **Event Handling**: User interactions and audio playback
- **Computed Values**: Automatic recalculation based on dependencies
- **Mood Algorithm**: Color theory applied to emotion detection

## 📂 Project Structure

```
mood-colors/
├── src/
│   ├── App.svelte          # Main component
│   └── main.js             # Entry point
├── public/
├── index.html
├── package.json
├── vite.config.js
└── README.md
```

## 🎵 How the Sounds Work

The app uses the **Web Audio API** to generate sounds programmatically:

- Each mood has a unique frequency and waveform
- No external audio files needed
- Sounds are generated in real-time using oscillators

## 🧮 Mood Detection Algorithm

The mood is determined by:

1. **Color Ratios**: Percentage of each RGB component
2. **Brightness**: Perceived luminosity (weighted formula)
3. **Combinations**: Specific patterns trigger specific moods

Example:

- High red ratio + high brightness = 🔥 Energetic
- High blue ratio + low brightness = 🌙 Mysterious

**Made with ❤️ using Svelte**
