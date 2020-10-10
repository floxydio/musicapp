<template>
  <div id="app">
    <header>
      <h1>My Music</h1>
    </header>
    <main>
      <section class="player">
        <h1 class="now">Now You're Playing Dio Favorite Song</h1>
        <h2 class="song-title">
          {{ current.title }} - <span>{{ current.artis }}</span>
        </h2>
        <div class="control">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>The Playlist</h3>
        <button
          v-for="song in songs"
          :key="song.src"
          @click="play(song)"
          :class="song.src == current.src ? 'song playing' : 'song'"
        >
          {{ song.title }} - {{ song.artis }}
        </button>
      </section>
      <HelloWorld msg="Welcome To Vue" />
    </main>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "app",
  components: {
    HelloWorld,
  },
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: "Manusia Bodoh",
          artis: "Ada Band",
          src: require("./assets/ADA Band - Manusia Bodoh.mp3"),
        },
        {
          title: "Ada Apa Denganmu",
          artis: "Peterpan",
          src: require("./assets/Peterpan - Ada Apa Denganmu.mp3"),
        },
        {
          title: "Hope",
          artis: "The Chainsmoker",
          src: require("./assets/The Chainsmokers - Hope ft. Winona Oak (Lyric Video).mp3"),
        },
        {
          title: "My Type",
          artis: "The Chainsmoker",
          src: require("./assets/The Chainsmokers - My Type (Audio) ft. Emily Warren.mp3"),
        },
        {
          title: "Paris",
          artis: "The Chainsmoker",
          src: require("./assets/The Chainsmokers - Paris (Lyric).mp3"),
        },
      ],
      player: new Audio(),
    };
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;

        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener(
        "ended",
        function () {
          this.index++;
          if (this.index > this.songs.length - 1) {
            this.index = 0;
          }
          this.current = this.songs[this.index];
          this.play(this.current);
        }.bind(this)
      );
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev() {
      if (this.index < 0) {
        this.index = this.song.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },
};
</script>


<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
}
header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: #212121;
}
.player .now {
  font-size: 20px;
  margin-bottom: 50px;
  font-weight: 500;
}
header h1 {
  color: white;
}
main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}
.song-title {
  color: #212121;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
  margin-bottom: 1rem;
}
.song-title span {
  font-weight: 400;
  font-style: italic;
}

.control {
  display: flex;
  justify-content: center;
  padding: 30px 15;
}

button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}

.play,
.pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #fff;
  background: #cc2e5d;
}
button:hover {
  opacity: 0.8;
}
.next,
.prev {
  font-size: 15px;
  font-weight: 700;
  padding: 10px 30px;
  margin: 0px 20px;
  border-radius: 6px;
  color: #fff;
  background: #cc2e5d;
}
.playlist {
  margin-top: 1.5rem;
  border: 1px solid black;
}
.playlist h3 {
  color: #212121;
  font-size: 20px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}
.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}
.playlist .song:hover {
  text-decoration: underline;
}
.playlist,
.song .playing {
  color: #fff;
  background: white;
  border-radius: 20px;
}
</style>