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

      //Question: Why not just call the next() method directly??
      this.player.addEventListener('ended', function(){
        this.index++
        
        if(this.index > this.songs.length - 1)
          this.index = 0
      
        this.current = this.songs[this.index]
        this.play(this.current)
      }.bind(this))

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
main{
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}

.song-title{
  color: #212121;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}
.song-title span{
  font-weight: 400;
  font-style: italic;
  color: #962121;
}

.controls{
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;
}

button{
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}

button:hover{
  opacity: 0.8;
}

.play, .pause{
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: white;
  background-color: rgb(219, 39, 78);
}

.next, .prev{
  font-size: 16px;
  font-weight: 700;
  padding: 12px 20px;
  margin: 0px 15px;
  border-radius: 8px;
  color: white;
  background-color: rgb(185, 32, 65);
}

.playlist{
  padding: 0px 30px;
}
.playlist h3{
  color: #212121;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}
.playlist .song{
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}

.playlist .song:hover{
  color: #FF5858;
}

.playlist .song.playing{
  color: white;
  background: linear-gradient(to right, #CC2E5D, #FF5858);
}

</style>
