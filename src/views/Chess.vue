<template>
  <div class="chess">
      <h1>中&nbsp;&nbsp;国&nbsp;&nbsp;象&nbsp;&nbsp;棋</h1>
      <div class="chessBoard" @click="imgClick($event)">
        <img id="BR1" src="../assets/chesses/BR.png"/>
        <img id="BN1" src="../assets/chesses/BN.png"/>
        <img id="BB1" src="../assets/chesses/BB.png"/>
        <img id="BA1" src="../assets/chesses/BA.png"/>
        <img id="BK" src="../assets/chesses/BK.png"/>
        <img id="BA2" src="../assets/chesses/BA.png"/>
        <img id="BB2" src="../assets/chesses/BB.png"/>
        <img id="BN2" src="../assets/chesses/BN.png"/>
        <img id="BR2" src="../assets/chesses/BR.png"/>
        <img id="BC1" src="../assets/chesses/BC.png"/>
        <img id="BC2" src="../assets/chesses/BC.png"/>
        <img id="BP1" src="../assets/chesses/BP.png"/>
        <img id="BP2" src="../assets/chesses/BP.png"/>
        <img id="BP3" src="../assets/chesses/BP.png"/>
        <img id="BP4" src="../assets/chesses/BP.png"/>
        <img id="BP5" src="../assets/chesses/BP.png"/>

        <img id="RP1" src="../assets/chesses/RP.png"/>
        <img id="RP2" src="../assets/chesses/RP.png"/>
        <img id="RP3" src="../assets/chesses/RP.png"/>
        <img id="RP4" src="../assets/chesses/RP.png"/>
        <img id="RP5" src="../assets/chesses/RP.png"/>
        <img id="RC1" src="../assets/chesses/RC.png"/>
        <img id="RC2" src="../assets/chesses/RC.png"/>
        <img id="RR1" src="../assets/chesses/RR.png"/>
        <img id="RN1" src="../assets/chesses/RN.png"/>
        <img id="RB1" src="../assets/chesses/RB.png"/>
        <img id="RA1" src="../assets/chesses/RA.png"/>
        <img id="RK" src="../assets/chesses/RK.png"/>
        <img id="RA2" src="../assets/chesses/RA.png"/>
        <img id="RB2" src="../assets/chesses/RB.png"/>
        <img id="RN2" src="../assets/chesses/RN.png"/>
        <img id="RR2" src="../assets/chesses/RR.png"/>
      </div>
  </div>
</template>

<script>
export default{
  data () {
    return {
      selected: {},
      chessWidth: 40,
      chessHeight: 40,
      riverDimen: 208,
    }
  },
  mounted() {
  },
  methods: {
    formatPos: function(pos) {
      pos.x = Math.round(pos.x / 40 - 1) * 40 + 8
      pos.y = Math.round(pos.y / 40 - 1) * 40 + 8
      return pos
    },
    validMove: function(id, from, to) {
      switch(id.slice(0, 2)) {
        case "RP":
          console.log(from, to)
          if(to.x === from.x && from.y - to.y === 40) {
            return true
          } else if (to.y === from.y && Math.abs(to.x - from.x) === 40) {
            if(from.y < this.riverDimen) {
              return true
            }
          }
          return false
        case "RC":
          console.log()
        case "RR":
          console.log()
        case "RN":
          console.log()
        case "RB":
          console.log()
        case "RA":
          console.log()
        case "RK":
          console.log()
      }
      return true
    },
    move: function(target, from, to) {
      console.log(target.style)
      // document.getElementById(target.id).setAttribute("style", "top:"+to.y+"px;left:"+to.x+"px")
      target.style.top = to.y+"px"
      target.style.left = to.x+"px"
      target.classList.add("slider")
    },
    imgClick: function(event) {
      if(event.target.id.indexOf("R") === 0) {
        if(event.target.className.indexOf("OOS") == -1) {
          this.selected.className = ""
          event.target.className = "OOS"
          this.selected = event.target
        } else {
          event.target.className = ""
        }
      } else if(event.target.id === "") {
        switch(this.selected.id) {
          // 兵
          case "RP1":
          case "RP2":
          case "RP3":
          case "RP4":
          case "RP5":
            var from = {"x": this.selected.offsetLeft, "y": this.selected.offsetTop}
            var to = this.formatPos({"x": event.layerX, "y": event.layerY})
            if(this.validMove(this.selected.id, from, to)) {
              console.log(123123)
              this.move(this.selected, from, to)
              this.selected.className = ""
              this.selected = {}
            }
            break;
          // 砲
          case "RC1":
          case "RC2":
            break;

          // 車
          case "RR1":
          case "RR2":
            break;
          // 馬
          case "RN1":
          case "RN2":
            break;
          // 相
          case "RB1":
          case "RB2":
            break;
          // 仕
          case "RA1":
          case "RA2":
            break;
          // 帥
          case "RK":
            break;
          default:
        }
      }
    }
  }
}
</script>
<style scoped>
  .chess{
    margin: auto;
    width: 377px;
    height: 600px;
  }
  .chessBoard {
    position: relative;
    margin: auto;
    width: 377px;
    height: 417px;
    background: url("../assets/chessBoard.png") no-repeat;
  }
  .slider{
    transition-property: all;
    transition-duration: .5s;
    transition-timing-function: cubic-bezier(0, 1, 0.5, 1);
  }
  img {
    position: absolute;
  }
  .OOS {
    background: url("../assets/chesses/OOS.png")
  }
  #BP1 {
    top: 128px;
    left: 8px;
  }
  #BP2 {
    top: 128px;
    left: 88px;
  }
  #BP3 {
    top: 128px;
    left: 168px;
  }
  #BP4 {
    top: 128px;
    right: 88px;
  }
  #BP5 {
    top: 128px;
    right: 8px;
  }
  #BC1 {
    top: 88px;
    left: 48px;
  }
  #BC2 {
    top: 88px;
    right: 48px;
  }
  #BR1 {
    top: 8px;
    left: 8px;
  }
  #BR2 {
    top: 8px;
    right: 8px;
  }
  #BN1 {
    top: 8px;
    left: 48px;
  }
  #BN2 {
    top: 8px;
    right: 48px;
  }
  #BB1 {
    top: 8px;
    left: 88px;
  }
  #BB2 {
    top: 8px;
    right: 88px;
  }
  #BA1 {
    top: 8px;
    left: 128px;
  }
  #BA2 {
    top: 8px;
    right: 128px;
  }
  #BK {
    top: 8px;
    left: 168px;
  }

  #RP1 {
    bottom: 128px;
    left: 8px;
  }
  #RP2 {
    bottom: 128px;
    left: 88px;
  }
  #RP3 {
    bottom: 128px;
    left: 168px;
  }
  #RP4 {
    bottom: 128px;
    right: 88px;
  }
  #RP5 {
    bottom: 128px;
    right: 8px;
  }
  #RC1 {
    bottom: 88px;
    left: 48px;
  }
  #RC2 {
    bottom: 88px;
    right: 48px;
  }
  #RR1 {
    bottom: 8px;
    left: 8px;
  }
  #RR2 {
    bottom: 8px;
    right: 8px;
  }
  #RN1 {
    bottom: 8px;
    left: 48px;
  }
  #RN2 {
    bottom: 8px;
    right: 48px;
  }
  #RB1 {
    bottom: 8px;
    left: 88px;
  }
  #RB2 {
    bottom: 8px;
    right: 88px;
  }
  #RA1 {
    bottom: 8px;
    left: 128px;
  }
  #RA2 {
    bottom: 8px;
    right: 128px;
  }
  #RK {
    bottom: 8px;
    left: 168px;
  }
</style>