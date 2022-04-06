<script>
import axios from 'axios'
export default {
  data: function () {
    return {
      message: "Music App",
      songs: [],
      newSong: {},
      currentSong: {}
    };
  },
  created: function () {
    this.indexSongs();
  },
  methods: {
    indexSongs: function () {
      axios.get('/songs').then(
        response => {
          console.log(response.data);
          this.songs = response.data;
        }
      )
    },
    createSong: function () {
      axios.post('/songs', this.newSong).then(
        response => {
          console.log(response.data);
          this.songs.push(response.data);
          this.newSong = {}
        }
      )
    },
    showSong: function (song) {
      this.currentSong = song;
      console.log(this.currentSong);
      document.querySelector("#song-details").showModal();
    },
    sortByTitle: function () {
      this.songs = this.songs.sort((a, b) => (a.title > b.title) ? 1 : -1);
      console.log(this.songs);
    },
    sortByArtist: function () {
      this.songs = this.songs.sort((a, b) => (a.artist > b.artist) ? 1 : -1);
    }
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>Song Title:
    <input type="text" v-model="newSong.title" />
    <br />Artist:
    <input type="text" v-model="newSong.artist" />
    <br />Album:
    <input type="text" v-model="newSong.album" />
    <br />Year:
    <input v-model="newSong.year" />
    <br />
    <button v-on:click="createSong()">Add Song</button>
    <br />
    <button v-on:click="sortByTitle()">Sort by title</button>
    <button v-on:click="sortByArtist()">Sort by artist</button>
    <br />
    <ul>
      <li v-for="song in songs">
        <button v-on:click="showSong(song)">Details</button>
        {{ song.title }} &#8211; {{ song.artist }}
      </li>
    </ul>
  </div>

  <dialog id="song-details">
    <form method="dialog">
      <h2>{{ currentSong.title }}</h2>
      <p>Artist: {{ currentSong.artist }}</p>
      <p>Album: {{ currentSong.album }}</p>
      <p>Year: {{ currentSong.year }}</p>
      <button>Close</button>
    </form>
  </dialog>
</template>

<style>
.home {
  text-align: left;
}

h1,
h2,
h3,
h4 {
  text-align: center;
}
</style>