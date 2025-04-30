<template>
  <div class="app">
    <!-- PAGE 1 -->
    <div v-if="step === 1">
      <div class="header-wrapper">
        <h1 class="header">
          <span class="vgm glitch">VGM</span>
          <span class="title-text glitch">-Percussive Isolator</span>
        </h1>
      </div>

      <div class="main-grid">
        <!-- Upload Box -->
        <label class="upload-box">
          <input type="file" style="display: none" @change="handleUpload" />
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="70px" height="70px">
            <path fill="black" d="M5 20h14v-2H5v2zm7-18l-7 7h4v4h6v-4h4l-7-7z"/>
          </svg>
          <div class="upload-label">Upload a file</div>
        </label>

        <div class="or-text">OR</div>

        <!-- Genre Panel -->
        <div class="genre-column">
          <button class="arrow-button" @click="scrollGenres(-1)" :disabled="genreStart === 0">▲</button>
          <button
            v-for="g in visibleGenres"
            :key="g"
            @click="selectGenre(g)"
            :class="['genre-btn', g.toLowerCase(), { selected: selectedGenre === g }]"
          >
            {{ g }} {{ emoji[g] || '' }}
          </button>
          <button class="arrow-button" @click="scrollGenres(1)" :disabled="genreStart + 4 >= genres.length">▼</button>
          <button class="ok-btn" :class="okButtonClass" :disabled="!canProceed" @click="step = 2">OK!</button>
        </div>

        <div class="vertical-label">Select A Genre!</div>
      </div>
    </div>

    <!-- PAGE 2 -->
    <div v-else-if="step === 2" class="page-two">
      <button class="start-over-btn spin-hover" @click="reset">
        <div class="arrow">⬅</div>
        <div class="text">Start<br />Over!</div>
      </button>
      <div class="track-info">
        <h2 class="black-text">Mystery Stage Theme</h2>
        <p class="black-text">01:34 / 03:20</p>
      </div>
      <div class="audio-player">
        <audio controls class="audio-bar">
          <source :src="audioUrl" />
        </audio>
      </div>
      <div class="actions">
        <button class="isolate-btn" @click="step = 3">Isolate Music!</button>
        <button class="isolate-btn" @click="step = 3">Isolate Drums!</button>
      </div>
    </div>

    <!-- PAGE 3 -->
    <div v-else-if="step === 3" class="page-three">
      <button class="start-over-btn spin-hover" @click="reset">
        <div class="arrow">⬅</div>
        <div class="text">Start<br />Over!</div>
      </button>
      <h2 class="title-text glitch">Percussive Isolation Complete!</h2>
      <div class="audio-comparison">
        <div>
          <p class="black-text">Old Track:</p>
          <audio controls class="audio-bar"><source :src="audioUrl" /></audio>
        </div>
        <div>
          <p class="black-text">New Isolated Track:</p>
          <audio controls class="audio-bar"><source :src="audioUrl" /></audio>
        </div>
      </div>
      <button class="download-btn">Download File ⬇</button>
    </div>

    <footer class="footer">Site Powered by Kc Weaver</footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      step: 1,
      selectedGenre: null,
      audioUrl: null,
      genreStart: 0,
      genres: [
        'Beach', 'Volcano', 'Magical', 'Desert',
        'Space', 'Forest', 'Underwater', 'City'
      ],
      emoji: {
        Beach: '🐚',
        Volcano: '🌋',
        Magical: '🪄',
        Desert: '🦂',
        Space: '🚀',
        Forest: '🌲',
        Underwater: '🐠',
        City: '🌆'
      }
    };
  },
  computed: {
    canProceed() {
      return this.audioUrl || this.selectedGenre;
    },
    visibleGenres() {
      return this.genres.slice(this.genreStart, this.genreStart + 4);
    },
    okButtonClass() {
      if (this.selectedGenre) return this.selectedGenre.toLowerCase();
      if (this.audioUrl) return 'rainbow';
      return '';
    }
  },
  methods: {
    selectGenre(genre) {
      this.selectedGenre = genre;
    },
    handleUpload(e) {
      const file = e.target.files[0];
      if (file) {
        this.audioUrl = URL.createObjectURL(file);
        if (!this.selectedGenre) this.selectedGenre = null;
      }
    },
    scrollGenres(direction) {
      const newStart = this.genreStart + direction;
      if (newStart >= 0 && newStart + 4 <= this.genres.length) {
        this.genreStart = newStart;
      }
    },
    reset() {
      this.step = 1;
      this.selectedGenre = null;
      this.audioUrl = null;
      this.genreStart = 0;
    }
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Nunito:wght@800&display=swap');

body {
  margin: 0;
  font-family: 'Nunito', sans-serif;
  background: linear-gradient(to bottom right, #ffeb3b, #fff176);
}

.app {
  min-height: 100vh;
  padding: 2rem;
  position: relative;
  box-sizing: border-box;
}

.black-text {
  color: black;
}

.header-wrapper {
  max-width: 960px;
  margin: 0 auto;
  padding-top: 2.5rem;
  margin-left: 120px;
}

.header {
  display: flex;
  align-items: baseline;
  font-size: 2.5rem;
  font-weight: 900;
  gap: 0.5rem;
  line-height: 1;
}

.vgm {
  color: #f3b900;
  text-decoration: underline;
}

.title-text {
  color: black;
}

.glitch {
  text-shadow:
    0 0 2px #000,
    -2px 0 #ff00ff,
    2px 1px #00ffff,
    1px -1px #9900ff,
    -1px 2px #c080ff;
}

.main-grid {
  max-width: 1000px;
  margin: 3rem auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  position: relative;
}

.upload-box {
  width: 250px;
  height: 250px;
  border-radius: 36px;
  background-color: #ccc;
  border: 3px solid black;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  box-shadow: 3px 3px rgba(0, 0, 0, 0.3);
  cursor: pointer;
}

.upload-label {
  margin-top: 1rem;
  font-size: 0.9rem;
  font-weight: bold;
  color: #333;
}

.or-text {
  font-size: 2.4rem;
  font-weight: 900;
  color: black;
  margin: 0 2rem;
}

.genre-column {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.arrow-button {
  width: 55px;
  height: 55px;
  font-size: 2rem;
  color: #78e2e7;
  background: white;
  border: 3px solid black;
  border-radius: 50%;
  margin: 0.4rem 0;
  box-shadow: 2px 2px black;
  cursor: pointer;
  transition: transform 0.2s ease;
}

.arrow-button:hover:not(:disabled) {
  filter: brightness(0.95);
  transform: scale(1.05);
}

.arrow-button:disabled {
  opacity: 0.3;
  pointer-events: none;
  box-shadow: none;
}

.genre-btn {
  width: 180px;
  font-size: 1.2rem;
  font-weight: bold;
  padding: 12px 0;
  margin: 0.5rem 0;
  border: 3px solid black;
  border-radius: 12px;
  box-shadow: 3px 3px black;
  cursor: pointer;
  text-align: center;
  background-color: white;
  transition: filter 0.2s;
}

.genre-btn:hover {
  filter: brightness(0.95);
}

.genre-btn.beach { background-color: #b2ebf2; }
.genre-btn.volcano { background-color: #f8bbd0; }
.genre-btn.magical { background-color: #e1bee7; }
.genre-btn.desert { background-color: #fcb43a; }
.genre-btn.space { background-color: #d1c4e9; }
.genre-btn.forest { background-color: #c8e6c9; }
.genre-btn.underwater { background-color: #b3e5fc; }
.genre-btn.city { background-color: #d7ccc8; }

.genre-btn.selected {
  border: 3px solid black;
}

.ok-btn {
  font-size: 1.5rem;
  font-weight: 900;
  padding: 12px 28px;
  margin-top: 1.2rem;
  border-radius: 12px;
  border: 3px solid black;
  box-shadow: 3px 3px black;
  cursor: pointer;
  transition: filter 0.2s;
}

.ok-btn:hover:not(:disabled) {
  filter: brightness(0.95);
}

.ok-btn:disabled {
  background: #aaa;
  cursor: not-allowed;
  box-shadow: none;
}

.ok-btn.beach { background-color: #b2ebf2; }
.ok-btn.volcano { background-color: #f8bbd0; }
.ok-btn.magical { background-color: #e1bee7; }
.ok-btn.desert { background-color: #fcb43a; }
.ok-btn.space { background-color: #d1c4e9; }
.ok-btn.forest { background-color: #c8e6c9; }
.ok-btn.underwater { background-color: #b3e5fc; }
.ok-btn.city { background-color: #d7ccc8; }

.ok-btn.rainbow {
  background: linear-gradient(45deg, red, orange, yellow, green, blue, indigo, violet);
  background-size: 400% 400%;
  animation: rainbow-fade 3s ease infinite;
  color: black;
}

@keyframes rainbow-fade {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

.vertical-label {
  position: absolute;
  right: -130px;
  top: 50%;
  transform: rotate(90deg) translateX(-50%) translateY(-50%);
  font-size: 2rem;
  font-weight: 900;
  color: black;
  white-space: nowrap;
}

.page-two, .page-three {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 4rem;
}

.track-info {
  margin-bottom: 1rem;
  text-align: center;
}

.audio-player {
  margin-bottom: 2rem;
}

.audio-bar {
  width: 500px;
  height: 40px;
}

.actions {
  display: flex;
  gap: 2rem;
}

.audio-comparison {
  display: flex;
  flex-direction: column;
  gap: 2rem;
  align-items: center;
  margin-bottom: 2rem;
}

.isolate-btn, .download-btn {
  background: #b2f7ef;
  border: 3px solid black;
  font-size: 1.5rem;
  font-weight: bold;
  padding: 1rem 2rem;
  border-radius: 12px;
  box-shadow: 3px 3px black;
  cursor: pointer;
  transition: filter 0.2s;
}

.isolate-btn:hover, .download-btn:hover {
  filter: brightness(0.95);
}

.start-over-btn {
  position: absolute;
  top: 1rem;
  left: 1rem;
  width: 90px;
  height: 90px;
  border-radius: 50%;
  background: #f9a44b;
  border: 2px solid black;
  font-weight: bold;
  font-size: 0.7rem;
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.start-over-btn .arrow {
  font-size: 1.6rem;
  margin-bottom: 4px;
  transition: transform 0.3s ease;
}

.spin-hover:hover .arrow {
  transform: rotate(-360deg);
}

.footer {
  position: absolute;
  bottom: 6px;
  left: 12px;
  font-size: 0.7rem;
  font-weight: 300;
  color: #333;
}
</style>