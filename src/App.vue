<template>
  <div id="app">
    <header>
      <h1>My Music</h1>
    </header>
    <main>
      <section class="players">

        <h2 class="song-title">
          {{ current.title }}
          <span> - {{ current.artist }}</span>
        </h2>

        <div class="controls">
          <button class="prev" @click="prev">Previous</button>
          <button class="play" v-if="!isPlaying" v-on:click="play">Play</button>
          <button class="pause" v-else v-on:click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>

      </section>

      <section class="playlist">
        <h3>The Playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{ song.title }}
          <span> - {{song.artist}} </span>
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',

  data(){
    return {
      current:{
        
      },
      index: 0,
      isPlaying: false,
      songs:[
        {
          title: 'Broken',
          artist: 'Neelansh',
          src: require('./assets/Broken.mp3')
        },
        {
          title: 'Show your Mettle',
          artist: 'Neelansh',
          src: require('./assets/Mettle.mp3')
        }
      ],
      player: new Audio()
    }
  },

  methods:{
    play(song){
      if (typeof song.src != "undefined"){
        this.current = song
        this.player.src = this.current.src
      }
      this.player.play()
      this.isPlaying = true
    },

    pause(){
      this.player.pause()
      this.isPlaying = false
    },
    prev(){
      this.index--
      if(this.index < 0)
        this.index = this.songs.length - 1
      
      this.current = this.songs[this.index]
      this.play(this.current)
    },
    next(){
      this.index++;
      if(this.index > this.songs.length - 1)
        this.index = 0
      
      this.current = this.songs[this.index]
      this.play(this.current)
    }
  },

  created(){
    this.current = this.songs[this.index]
    this.player.src = this.current.src
  }

}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: sans-serif;
}
header{
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  color: white;
  background: #212121;
}
span{
  color: #af2d2d;
}
</style>
