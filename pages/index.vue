<template>
  <div>
    <!-- <h2>The live started at </h2> -->
    <video 
    id= "video-player"
    class="cld-video-player"
    >
    </video> 
    <div class="overlay"  v-if = 'overlay'>
     
      The live has ended. 
    </div>
  </div>
</template>

<script>
  export default {
    data(){
      return{
        cld: null,
        player: null,
        video: "videoplayback_1_pr2hzi",
        duration: null,
        startTime: 1642870132613,
        currentTime: null,
        liveStartTime: null,
        overlay: false,
        controls: true
      }
    },
    methods: {
      onEnded(){
        this.overlay = true;
        this.controls = false
      }
    },

    mounted(){
      this.cld= cloudinary.Cloudinary.new({
        cloud_name:  "amarachi-2812",
        secure:  true
      })
      this.player = this.cld.videoPlayer(
        'video-player', {
          controls: this.controls,
          autoplay: true
        }
      )

      this.player.source(this.video);

      // Gets the duration of the video

      this.player.on('loadeddata', () => {
        this.duration = this.player.duration(); 
      })

      // Gets the current time at which the user opens the video

      const d = new Date ()
      this.currentTime = d.getTime();

      // Gets the difference betwewn when the user opened the video and when the video started

      const difference = this.currentTime - this.startTime

      // Changes the time from millisecond to seconds.
      this.liveStartTime = difference / 1000;
      

      // Plays the video from a particular time

      this.player.on('play', ()=> {
       this.player.currentTime(this.liveStartTime);

      })

      // Controls the overlay

      this.player.on('ended', ()=> {
        this.onEnded()
      })

      if (this.liveStartTime > this.duration){
        this.onEnded()
      }

     
      
    }

  }


</script>

<style scoped>

@import url('https://fonts.googleapis.com/css2?family=Nunito:wght@300&display=swap');


div{
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: 'Nunito', sans-serif;

}
h2{
  font-size: 20px;
}
.cld-video-player{
  width: 500px;
  height: 500px;
}
.overlay{
  width: 500px;
  height: 500px;
  position: absolute;
  background-color: rgba(0,0,0, 0.70);
  color: #fff;
  font-size: 16px;
  top: 7px;
  bottom: 0px;
  
}


</style>
