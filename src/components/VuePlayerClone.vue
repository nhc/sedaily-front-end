<template>
    <div class="aplayer"></div>
</template>
<script type="text/babel">
import APlayer from 'aplayer'
export default {
  props: {
    narrow: {
      type: Boolean,
      default: false
    },
    autoplay: {
      type: Boolean,
      default: false
    },
    showlrc: {
      type: Number,
      default: 0
    },
    mutex: {
      type: Boolean,
      default: false
    },
    theme: {
      type: String,
      default: '#b7daff'
    },
    mode: {
      type: String,
      default: 'circulation'
    },
    preload: {
      type: String,
      default: 'auto'
    },
    listmaxheight: String,
    music: {
      type: [Object, Array],
      required: true,
      validator (value) {
        let songs

        if (!(value instanceof Array)) {
          songs = [value]
        } else {
          songs = value
        }
        for (let i = 0; i < songs.length; i++) {
          let song = songs[i]
          if (!song.url || !song.title || !song.author) {
            song.title = song.title || 'Untitled'
            song.author = song.author || 'Unknown'
            return false
          }
        }
        return true
      }
    }
  },
  data () {
    return {
      control: null
    }
  },
  watch: {
    music (music) {
      if (!music.url || music.url === null) {
        return
      }

      if (!music.pic || music.pic === null) {
        return
      }

      if (this.control) {
        this.control.destroy()
      }

      let player = this.control = new APlayer({
        element: this.$el,
        narrow: this.narrow,
        autoplay: this.autoplay,
        showlrc: this.showlrc,
        mutex: this.mutex,
        theme: this.theme,
        preload: this.preload,
        mode: this.mode,
        listmaxheight: this.listmaxheight,
        music: this.music
      })
      this.control.setMusic(0)

      player.on('play', () => {
        this.$emit('play')
      })
      player.on('pause', () => {
        this.$emit('pause')
      })
      player.on('canplay', () => {
        this.$emit('canplay')
      })
      player.on('playing', () => {
        this.$emit('playing')
      })
      player.on('ended', () => {
        this.$emit('ended')
      })
      player.on('error', () => {
        this.$emit('error')
      })

      if (this.autoplay) {
        this.control.play()
      }

      this.$emit('change', music)
    }
  },
  mounted () {
    if (!this.music.url || this.music.url === null) {
      return
    }
    let player = this.control = new APlayer({
      element: this.$el,
      narrow: this.narrow,
      autoplay: this.autoplay,
      showlrc: this.showlrc,
      mutex: this.mutex,
      theme: this.theme,
      preload: this.preload,
      mode: this.mode,
      listmaxheight: this.listmaxheight,
      music: this.music
    })
    player.on('play', () => {
      this.$emit('play')
    })
    player.on('pause', () => {
      this.$emit('pause')
    })
    player.on('canplay', () => {
      this.$emit('canplay')
    })
    player.on('playing', () => {
      this.$emit('playing')
    })
    player.on('ended', () => {
      this.$emit('ended')
    })
    player.on('error', () => {
      this.$emit('error')
    })
  }
}
</script>
