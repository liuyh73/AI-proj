<template>
  <div class="chess">
      <h1>中&nbsp;&nbsp;国&nbsp;&nbsp;象&nbsp;&nbsp;棋</h1>
      <div class="chessBoard" @click="turnToPlayer && imgClick($event)">
        <img id="BR1" ref="BR1" src="../assets/chesses/BR.png"/>
        <img id="BN1" ref="BN1" src="../assets/chesses/BN.png"/>
        <img id="BB1" ref="BB1" src="../assets/chesses/BB.png"/>
        <img id="BA1" ref="BA1" src="../assets/chesses/BA.png"/>
        <img id="BK" ref="BK" src="../assets/chesses/BK.png"/>
        <img id="BA2" ref="BA2" src="../assets/chesses/BA.png"/>
        <img id="BB2" ref="BB2" src="../assets/chesses/BB.png"/>
        <img id="BN2" ref="BN2" src="../assets/chesses/BN.png"/>
        <img id="BR2" ref="BR2" src="../assets/chesses/BR.png"/>
        <img id="BC1" ref="BC1" src="../assets/chesses/BC.png"/>
        <img id="BC2" ref="BC2" src="../assets/chesses/BC.png"/>
        <img id="BP1" ref="BP1" src="../assets/chesses/BP.png"/>
        <img id="BP2" ref="BP2" src="../assets/chesses/BP.png"/>
        <img id="BP3" ref="BP3" src="../assets/chesses/BP.png"/>
        <img id="BP4" ref="BP4" src="../assets/chesses/BP.png"/>
        <img id="BP5" ref="BP5" src="../assets/chesses/BP.png"/>

        <img id="RP1" ref="RP1" src="../assets/chesses/RP.png"/>
        <img id="RP2" ref="RP2" src="../assets/chesses/RP.png"/>
        <img id="RP3" ref="RP3" src="../assets/chesses/RP.png"/>
        <img id="RP4" ref="RP4" src="../assets/chesses/RP.png"/>
        <img id="RP5" ref="RP5" src="../assets/chesses/RP.png"/>
        <img id="RC1" ref="RC1" src="../assets/chesses/RC.png"/>
        <img id="RC2" ref="RC2" src="../assets/chesses/RC.png"/>
        <img id="RR1" ref="RR1" src="../assets/chesses/RR.png"/>
        <img id="RN1" ref="RN1" src="../assets/chesses/RN.png"/>
        <img id="RB1" ref="RB1" src="../assets/chesses/RB.png"/>
        <img id="RA1" ref="RA1" src="../assets/chesses/RA.png"/>
        <img id="RK" ref="RK" src="../assets/chesses/RK.png"/>
        <img id="RA2" ref="RA2" src="../assets/chesses/RA.png"/>
        <img id="RB2" ref="RB2" src="../assets/chesses/RB.png"/>
        <img id="RN2" ref="RN2" src="../assets/chesses/RN.png"/>
        <img id="RR2" ref="RR2" src="../assets/chesses/RR.png"/>
      </div>
  </div>
</template>

<script>
export default{
  data () {
    return {
      INT_MAX: 99999999999,
      INT_MIN: -1*99999999999,
      turnToPlayer: true,
      selected: null,
      chessWidth: 40,
      chessHeight: 40,
      redRiverDimen: 5,
      redJiuGongLeft: 3,
      redJiuGongRight: 5,
      redJiuGongTop: 7,
      blackRiverDimen: 4,
      blackJiuGongLeft: 3,
      blackJiuGongRight: 5,
      blackJiuGongTop: 2,
      red: "R",
      black: "B",
      bestStep: {},
      chessBoard: [
        ["BR1","BN1","BB1","BA1","BK","BA2","BB2","BN2","BR2"],
        [0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0,"BC1", 0, 0, 0, 0, 0,"BC2", 0],
        ["BP1", 0,"BP2", 0,"BP3", 0,"BP4", 0,"BP5"],
        [0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0],
        ["RP1", 0,"RP2", 0,"RP3", 0,"RP4", 0,"RP5"],
        [0,"RC1", 0, 0, 0, 0, 0,"RC2", 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0],
        ["RR1","RN1","RB1","RA1","RK","RA2","RB2","RN2","RR2"]
      ],
      chessValue: {
        "BR1": 500,
        "BN1": 300,
        "BB1": 200,
        "BA1": 100,
        "BK": this.INT_MAX,
        "BA2": 100,
        "BB2": 200,
        "BN2": 300,
        "BR2": 500,
        "BC1": 300,
        "BC2": 300,
        "BP1": 100,
        "BP2": 100,
        "BP3": 100,
        "BP4": 100,
        "BP5": 100,

        "RR1": 500,
        "RN1": 300,
        "RB1": 200,
        "RA1": 200,
        "RK": this.INT_MAX,
        "RA2": 100,
        "RB2": 200,
        "RN2": 300,
        "RR2": 500,
        "RC1": 300,
        "RC2": 300,
        "RP1": 100,
        "RP2": 100,
        "RP3": 100,
        "RP4": 100,
        "RP5": 100
      }
    }
  },
  mounted() {
  },
  methods: {
    min_max(a, b) {
      if(a<b) {
        return {"min": a, "max": b}
      }
      return {"min": b, "max": a}
    },
    formatPos: function(pos) {
      pos.x = Math.round((Math.round(pos.x / this.chessWidth - 1) * this.chessWidth + 8) / this.chessWidth)
      pos.y = Math.round((Math.round(pos.y / this.chessHeight - 1) * this.chessHeight + 8) / this.chessHeight)
      return pos
    },
    realPos: function(pos) {
      pos.x = pos.x * this.chessWidth + 8;
      pos.y = pos.y * this.chessHeight + 8;
      return pos
    },
    swap: function(chessBoard, from, to) {
      this.displayChessBoard(chessBoard)
      chessBoard[to.y][to.x] = this.chessBoard[from.y][from.x]
      chessBoard[from.y][from.x] = 0
      this.displayChessBoard(chessBoard)
    },
    displayChessBoard: function([...chessBoard]) {
      console.log(chessBoard)
    },
    getTarget: function(to) {
      let target = null
      if((to.y >= 0 && to.y < 10) && (to.x >= 0 && to.x < 9) && this.chessBoard[to.y][to.x]!=0) {
        target = this.$refs[this.chessBoard[to.y][to.x]]
      }
      return target
    },
    validMove: function(id, from, to) {
      if(id[0] === "R") {
        switch(id.slice(0, 2)) {
          // 兵
          case "RP":
            // console.log(from, to)
            if(to.x === from.x && from.y - to.y === 1) {
              return true
            } else if (to.y === from.y && Math.abs(to.x - from.x) === 1) {
              if(from.y < this.redRiverDimen) {
                return true
              }
            }
            return false
          // 砲
          case "RC":
            if(to.x == from.x || to.y == from.y) {
              let count = 0
              if(to.x == from.x) {
                let res = this.min_max(from.y, to.y)
                for(let i=res.min + 1; i<res.max; i++) {
                  if(this.chessBoard[i][to.x] != 0) {
                    count++
                  }
                }
              } else {
                let res = this.min_max(from.x, to.x)
                for(let i=res.min + 1; i<res.max; i++) {
                  if(this.chessBoard[from.y][i] != 0) {
                    count++
                  }
                }
              }
              if(this.getTarget(to) != null) {
                return count == 1
              } else {
                return count == 0
              }
            }
            return false
          // 車
          case "RR":
            if(to.x == from.x || to.y == from.y) {
              let count = 0
              if(to.x == from.x) {
                let res = this.min_max(from.y, to.y)
                for(let i=res.min + 1; i<res.max; i++) {
                  if(this.chessBoard[i][to.x] != 0) {
                    count++
                  }
                }
              } else {
                let res = this.min_max(from.x, to.x)
                for(let i=res.min + 1; i<res.max; i++) {
                  if(this.chessBoard[from.y][i] != 0) {
                    count++
                  }
                }
              }
              return count == 0
            }
            return false
          // 馬
          case "RN":
            // 顺时针：上、右、下、左
            // 上
            if(to.y == from.y-2 && (to.x == from.x-1 || to.x == from.x+1)) {
              return this.getTarget({"x": from.x, "y": from.y-1}) == null
            }
            // 右
            if(to.x == from.x+2 && (to.y == from.y-1 || to.y == from.y+1)) {
              return this.getTarget({"x": from.x+1, "y": from.y}) == null
            }
            // 下
            if(to.y == from.y+2 && (to.x == from.x+1 || to.x == from.x-1)) {
              return this.getTarget({"x": from.x, "y": from.y+1}) == null
            }
            // 左
            if(to.x == from.x-2 && (to.y == from.y+1 || to.y == from.y-1)) {
              return this.getTarget({"x": from.x-1, "y": from.y}) == null
            }
            return false
          // 相
          case "RB":
            // 不能过河
            if(to.y < this.redRiverDimen){
              return false
            }
            // 顺时针: 右上、右下、左下、左上
            // 右上
            if(to.y == from.y-2 && to.x == from.x+2) {
              return this.getTarget({"x": from.x+1, "y": from.y-1}) == null
            }
            // 右下
            if(to.y == from.y+2 && to.x == from.x+2) {
              return this.getTarget({"x": from.x+1, "y": from.y+1}) == null
            }
            // 左下
            if(to.y == from.y+2 && to.x == from.x-2) {
              return this.getTarget({"x": from.x-1, "y": from.y+1}) == null
            }
            // 左上
            if(to.y == from.y-2 && to.x == from.x-2) {
              return this.getTarget({"x": from.x-1, "y": from.y-1}) == null
            }
            return false
          // 仕
          case "RA":
            // 不能出九宫
            if(to.y < this.redJiuGongTop || to.x<this.redJiuGongLeft || to.x > this.redJiuGongRight){
              return false
            }
            // 顺时针：右上、右下、左下、左上
            // 右上
            if(to.y == from.y-1 && to.x == from.x+1) {
              return true
            }
            // 右下
            if(to.y == from.y+1 && to.x == from.x+1) {
              return true
            }
            // 左下
            if(to.y == from.y+1 && to.x == from.x-1) {
              return true
            }
            // 左上
            if(to.y == from.y-1 && to.x == from.x-1) {
              return true
            }
            return false
          // 帥
          case "RK":
            // 不能出九宫
            console.log(to, this.redJiuGongTop)
            if(to.y < this.redJiuGongTop || to.x<this.redJiuGongLeft || to.x > this.redJiuGongRight){
              return false
            }
            if(to.y == from.y && (to.x == from.x+1 || to.x == from.x-1)) {
              return true
            }
            if(to.x == from.x && (to.y == from.y+1 || to.y == from.y-1)) {
              return true
            }
            return false
        }
        return false
      } else if(id[0] === "B") {
        switch(id.slice(0, 2)) {
          // 兵
          case "BP":
            // console.log(from, to)
            if(to.x === from.x && from.y - to.y === -1) {
              return true
            } else if (to.y === from.y && Math.abs(to.x - from.x) === 1) {
              if(from.y > this.blackRiverDimen) {
                return true
              }
            }
            return false
          // 砲
          case "BC":
            if(to.x == from.x || to.y == from.y) {
              let count = 0
              if(to.x == from.x) {
                let res = this.min_max(from.y, to.y)
                for(let i=res.min + 1; i<res.max; i++) {
                  if(this.chessBoard[i][to.x] != 0) {
                    count++
                  }
                }
              } else {
                let res = this.min_max(from.x, to.x)
                for(let i=res.min + 1; i<res.max; i++) {
                  if(this.chessBoard[from.y][i] != 0) {
                    count++
                  }
                }
              }
              if(this.getTarget(to) != null) {
                if(this.getTarget(to).id[0] === this.red) {
                  return count == 1
                } else if(this.getTarget(to).id[0] === this.black) {
                  return false
                }
              } else {
                return count == 0
              }
            }
            return false
          // 車
          case "BR":
            if(to.x == from.x || to.y == from.y) {
              let count = 0
              if(to.x == from.x) {
                let res = this.min_max(from.y, to.y)
                for(let i=res.min + 1; i<res.max; i++) {
                  if(this.chessBoard[i][to.x] != 0) {
                    count++
                  }
                }
              } else {
                let res = this.min_max(from.x, to.x)
                for(let i=res.min + 1; i<res.max; i++) {
                  if(this.chessBoard[from.y][i] != 0) {
                    count++
                  }
                }
              }
              if(this.getTarget(to) != null) {
                if(this.getTarget(to).id[0] === this.red) {
                  return count == 0
                } else if(this.getTarget(to).id[0] === this.black) {
                  return false
                }
              } else {
                return count == 0
              }
            }
            return false
          // 馬
          case "RN":
            if(this.getTarget(to)!=null) {
              if(this.getTarget(to).id[0] === this.black) {
                return false
              }
            }
            // 顺时针：上、右、下、左
            // 上
            if(to.y == from.y-2 && (to.x == from.x-1 || to.x == from.x+1)) {
              return this.getTarget({"x": from.x, "y": from.y-1}) == null
            }
            // 右
            if(to.x == from.x+2 && (to.y == from.y-1 || to.y == from.y+1)) {
              return this.getTarget({"x": from.x+1, "y": from.y}) == null
            }
            // 下
            if(to.y == from.y+2 && (to.x == from.x+1 || to.x == from.x-1)) {
              return this.getTarget({"x": from.x, "y": from.y+1}) == null
            }
            // 左
            if(to.x == from.x-2 && (to.y == from.y+1 || to.y == from.y-1)) {
              return this.getTarget({"x": from.x-1, "y": from.y}) == null
            }
            return false
          // 相
          case "RB":
            // 不能过河
            if(to.y < this.redRiverDimen){
              return false
            }
            // 顺时针: 右上、右下、左下、左上
            // 右上
            if(to.y == from.y-2 && to.x == from.x+2) {
              return this.getTarget({"x": from.x+1, "y": from.y-1}) == null
            }
            // 右下
            if(to.y == from.y+2 && to.x == from.x+2) {
              return this.getTarget({"x": from.x+1, "y": from.y+1}) == null
            }
            // 左下
            if(to.y == from.y+2 && to.x == from.x-2) {
              return this.getTarget({"x": from.x-1, "y": from.y+1}) == null
            }
            // 左上
            if(to.y == from.y-2 && to.x == from.x-2) {
              return this.getTarget({"x": from.x-1, "y": from.y-1}) == null
            }
            return false
          // 仕
          case "RA":
            // 不能出九宫
            if(to.y < this.redJiuGongTop || to.x<this.redJiuGongLeft || to.x > this.redJiuGongRight){
              return false
            }
            // 顺时针：右上、右下、左下、左上
            // 右上
            if(to.y == from.y-1 && to.x == from.x+1) {
              return true
            }
            // 右下
            if(to.y == from.y+1 && to.x == from.x+1) {
              return true
            }
            // 左下
            if(to.y == from.y+1 && to.x == from.x-1) {
              return true
            }
            // 左上
            if(to.y == from.y-1 && to.x == from.x-1) {
              return true
            }
            return false
          // 帥
          case "RK":
            // 不能出九宫
            console.log(to, this.redJiuGongTop)
            if(to.y < this.redJiuGongTop || to.x<this.redJiuGongLeft || to.x > this.redJiuGongRight){
              return false
            }
            if(to.y == from.y && (to.x == from.x+1 || to.x == from.x-1)) {
              return true
            }
            if(to.x == from.x && (to.y == from.y+1 || to.y == from.y-1)) {
              return true
            }
            return false
        }
        return false
      }
    },
    move: function(selected, from, to) {
      let target = this.getTarget(to)
      this.swap(this.chessBoard, from, to)
      to = this.realPos(to)
      // console.log(selected.style)
      // document.getElementById(selected.id).setAttribute("style", "top:"+to.y+"px;left:"+to.x+"px")
      selected.style.top = to.y+"px"
      selected.style.left = to.x+"px"
      selected.style.transition = "all 0.5s"
      if(target != null) {
        setTimeout(() => {
          target.style.display = "none"
        }, 500);
      }
    },
    imgClick: function(event) {
      let from = {}, to = {} 
      if(this.selected == null && event.target.id.indexOf("R") === 0) {
        console.log("select")
        event.target.className = "OOS"
        this.selected = event.target
        return
      } else if (this.selected != null && event.target.id.indexOf("R") === 0) {
        console.log("change select")
        if(this.selected.id != event.target.id) {
          event.target.className = "OOS"
          this.selected.className = ""
          this.selected = event.target
        } else {
          this.selected.className = ""
          this.selected=null
        }
      } else if(this.selected != null && event.target.id.indexOf("B") === 0) {
        console.log("try to eat")
        from = this.formatPos({"x": this.selected.offsetLeft + this.chessWidth/2, "y": this.selected.offsetTop + this.chessWidth/2})
        to = this.formatPos({"x": event.target.offsetLeft + this.chessWidth/2, "y": event.target.offsetTop + this.chessHeight/2})
        if(this.validMove(this.selected.id, from, to)) {
          this.move(this.selected, from, to)
          this.selected.className = ""
          this.selected = null
          // this.robot()
        }
      } else if(this.selected != null) {
        console.log("move")
        from = this.formatPos({"x": this.selected.offsetLeft + this.chessWidth/2, "y": this.selected.offsetTop + this.chessWidth/2})
        to = this.formatPos({"x": event.layerX, "y": event.layerY})
        if(this.validMove(this.selected.id, from, to)) {
          this.move(this.selected, from, to)
          this.selected.className = ""
          this.selected = null
          // this.robot()
        }
      }
    },
    robot: function() {
      this.turnToPlayer = false
      console.log(this.generateNextStep(this.chessBoard, this.black, this.INT_MIN, this.INT_MAX, 1));
      this.move(this.$refs[this.bestStep.id], this.bestStep.from, this.bestStep.to)
    },
    evaluation: function([...chessBoard]) {
      return 100
    },
    genetatePStep: function([...chessBoard], from) {
      let tos = []
      // 上, 右, 下, 左
      let tosTemp = [{"x": from.x, "y": from.y-1}, {"x": from.x+1, "y": from.y}, {"x": from.x, "y": from.y+1}, {"x": from.x-1, "y": from.y}]
      let that = this
      tosTemp.forEach(function(to) {
        // console.log(chessBoard[from.y][from.x], to)
        if(that.validMove(chessBoard[from.y][from.x], from, to)) {
          tos.push(to)
        }
      })
      console.log("tos length: "+tos.length)
      return tos
    },
    generateNextStep: function([...chessBoard], turn, alpha, beta, step) {
      if(step >= 5) {
        return this.evaluation(chessBoard)
      }
      console.log("step"+step)
      console.log(chessBoard)
      let value, nextStep = {}
      for(let i=0; i<10; i++) {
        for(let j=0; j<9; j++) {
          if(chessBoard[i][j] == 0) {
            continue
          }
          if(chessBoard[i][j].indexOf(turn) === 0) {
            switch(chessBoard[i][j].slice(1, 2)) {
              case "P": 
                // 上右下左
                let [...tos] = this.genetatePStep(chessBoard, {"x": j, "y": i})
                console.log(tos.length)
                for(let i=0, len = tos.length; i<len; i++) {
                  this.swap(chessBoard, from, tos[i])
                  if(turn == this.black) {
                    value = this.generateNextStep(chessBoard, this.red, alpha, beta, step+1)
                    if(value > alpha) {
                      alpha = value
                      nextStep.id = chessBoard[i][j]
                      nextStep.from = from
                      nextStep.to = tos[i]
                    }
                    if(alpha > beta) {
                      return beta
                    }
                  } else {
                    value = this.generateNextStep(chessBoard, this.black, alpha, beta, step+1)
                    if(value < beta) {
                      beta = value
                      nextStep.id = chessBoard[i][j]
                      nextStep.from = from
                      nextStep.to = tos[i]
                    }
                    if(beta < alpha) {
                      return alpha
                    }
                  }
                  this.swap(chessBoard, tos[i], from)
                }
                break
              case "C":
              case "R":
              case "N":
              case "B":
              case "C":
              case "K":
            }
          }
        }
      }
      this.bestStep = nextStep
      console.log(nextStep)
      console.log("value:"+value)
      return value
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
    transition: all 0.5s;
    -webkit-transition: all 0.5s;
    /* transition-timing-function: cubic-bezier(0, 1, 0.5, 1); */
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