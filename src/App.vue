<template>
  <div id="app">
    <!--<HelloWorld msg="Welcome to Your Vue.js App"/>-->
    <h1>congarats</h1>
    <div id="playspace">
      <div class="player" v-for="player in playerX" 
      :key="player.num" 
      :style="{ top: player.y + 'px', left: player.x + 'px' }">
        <img class="char" src="./assets/temp/avatar.png"/>
        <img class="shadow" src="./assets/temp/avatar.png" :style="{ transform: calcShadow(player.x) }"/>
      </div>
    </div>
    <img id="floorL" src="./assets/temp/left.png"/>
    <img id="floor" src="./assets/temp/floor.png"/>
    <img id="floorR" src="./assets/temp/right.png"/>
    <img id="head" ref="sun" src="./assets/temp/head.png"/>
    <img id="sky" src="./assets/temp/sky.png"/>
  </div>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    //HelloWorld
  },
  data(){
    return {
      playerX: [{'num':0,'x':0,'y':0},{'num':0,'x':0,'y':0},{'num':0,'x':0,'y':0}],
      sunWidth: 0,
      screenWidth: 0,
    }
  },
  methods: {
    calcShadow(x){
      console.log(x)
      //let trueCenter = this.screenWidth / 2
      let outerZoneL = (this.screenWidth - this.sunWidth) / 2
      let outerZoneR = outerZoneL + this.sunWidth
      let drift
      if(x < outerZoneL){
        console.log('l')
        drift = -10
      }
      else if(x > outerZoneR){
        drift = 10
      }
      else{
        let placeOnSun = x - outerZoneL
        let percentComplete = placeOnSun / this.sunWidth
        //shifts drift from 0 to 1 scale to -10 to 10 scale
        drift = ((percentComplete * 2) - 1) * 10
      }
      // 0, 0 is center
      //10 far right, -10 far left
      return `rotate3d(10, ${drift}, ${drift}, -56deg) rotate(180deg)`
    },
  },
  mounted(){
    this.screenWidth = document.body.clientWidth;
    if(this.screenWidth < 592){
      this.sunWidth = this.screenWidth
    }
    else{
      this.sunWidth = 592
    }
    for(let i =0;i<this.playerX.length;i++){
      let X = Math.floor(Math.random() * this.screenWidth);
      let Y = Math.floor(Math.random() * 100);
      this.playerX[i].x = X
      this.playerX[i].y = Y
    }
  }
}
</script>

<style lang="sass" scoped>
@import url('https://fonts.googleapis.com/css2?family=Damion&display=swap')

#app
  max-width: 100%
  overflow: hidden
  h1
    font-family: 'Damion', cursive
    position: absolute
    bottom: 0
    margin: 0 auto
    left: 0
    right: 0
    text-align: center
    font-size: 4em
    padding-bottom: .25em
    z-index: 200
    pointer-events: none
  #playspace
    width: 100vw
    height: 22em
    position: absolute
    bottom: 0
    z-index: 5
    .player
      position: absolute
      img
        width: 5em
        position: absolute
      .shadow
        position: absolute
        top: 0
        opacity: 0.45
        -webkit-filter: brightness(0) blur(3px)
        filter: brightness(0) blur(3px)
        transform-origin: bottom
        pointer-events: none
  #sky
    position: absolute
    top: 0
    z-index: 0
    height: 613px
    min-width: 100%
  #head
    position: absolute
    z-index: 1
    bottom: 331px
    left: 0
    right: 0
    margin: 0 auto
    max-width: 100%
  #floor
    position: absolute
    z-index: 2
    bottom: 0
    height: 389px
    margin: 0 auto
    left: 0
    right: 0
    max-width: 100%
  #floorL
    left: 0
    position: absolute
    z-index: 2
    height: max-content
    width: calc((100vw - 705px)/2)
    bottom: 0
  #floorR
    @extend #floorL
    right: 0
    left: inherit
</style>
