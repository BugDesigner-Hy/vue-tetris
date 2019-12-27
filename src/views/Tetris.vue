<template>
  <div class="tetris">
    <div class="score">{{score}}</div>
    <div class="main">
      <div class="row" v-for="row in blocks" :key="Date.now() * Math.random()">
        <div
          class="diamond"
          v-for="diamond in row"
          :key="Date.now() * Math.random()"
          :style="{backgroundColor:getColor(diamond)}"
        ></div>
      </div>
    </div>
    <div class="panel">
      <svg class="icon down" aria-hidden="true" @click="down">
        <use xlink:href="#icon-zuobeifen-copy" />
      </svg>
      <svg class="icon left" aria-hidden="true" @click="left">
        <use xlink:href="#icon-left-copy" />
      </svg>
      <svg class="icon right" aria-hidden="true" @click="right">
        <use xlink:href="#icon-zuobeifen" />
      </svg>
      <svg class="icon rotate" aria-hidden="true" @click="rotate">
        <use xlink:href="#icon-xuanzhuan" />
      </svg>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      id: "",
      score: 0,
      next: [[0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0]],
      blocks: [
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 2, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 2, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 2, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
      ],
      type: 1,
      angle: 0,
      origin: [0, 3],
      shape: [
        //-
        [
          [[1, 1, 1, 1], [0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0]],
          [[0, 1, 0, 0], [0, 1, 0, 0], [0, 1, 0, 0], [0, 1, 0, 0]],
          [[1, 1, 1, 1], [0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0]],
          [[0, 1, 0, 0], [0, 1, 0, 0], [0, 1, 0, 0], [0, 1, 0, 0]]
        ],
        //T
        [
          [[0, 1, 0], [1, 1, 1], [0, 0, 0]],
          [[0, 1, 0], [0, 1, 1], [0, 1, 0]],
          [[0, 0, 0], [1, 1, 1], [0, 1, 0]],
          [[0, 1, 0], [1, 1, 0], [0, 1, 0]]
        ],
        //Z
        [
          [[0, 1, 1], [1, 1, 0], [0, 0, 0]],
          [[0, 1, 0], [0, 1, 1], [0, 0, 1]],
          [[0, 0, 0], [0, 1, 1], [1, 1, 0]],
          [[1, 0, 0], [1, 1, 0], [0, 1, 0]]
        ],
        //fan Z
        [
          [[1, 1, 0], [0, 1, 1], [0, 0, 0]],
          [[0, 0, 1], [0, 1, 1], [0, 1, 0]],
          [[0, 0, 0], [1, 1, 0], [0, 1, 1]],
          [[0, 1, 0], [1, 1, 0], [1, 0, 0]]
        ],
        //L
        [
          [[1, 0, 0], [1, 1, 1], [0, 0, 0]],
          [[0, 1, 1], [0, 1, 0], [0, 1, 0]],
          [[0, 0, 0], [1, 1, 1], [0, 0, 1]],
          [[0, 1, 0], [0, 1, 0], [1, 1, 0]]
        ],
        //fan L
        [
          [[0, 0, 1], [1, 1, 1], [0, 0, 0]],
          [[0, 1, 0], [0, 1, 0], [0, 1, 1]],
          [[0, 0, 0], [1, 1, 1], [1, 0, 0]],
          [[1, 1, 0], [0, 1, 0], [0, 1, 0]]
        ],
        //田
        [[[1, 1], [1, 1]], [[1, 1], [1, 1]], [[1, 1], [1, 1]], [[1, 1], [1, 1]]]
      ],
      randomColor: "",
      color: [
        "#fff176",
        "#ffd54f",
        "#ff8a65",
        "#dce775",
        "#aed581",
        "#4dd0e1",
        "#4fc3f7",
        "#f06292"
      ]
    };
  },
  created() {
    var _this = this;
    document.addEventListener("keydown", _this.control);
  },
  destroyed() {
    var _this = this;
    document.removeEventListener("keydown", _this.control);
  },
  mounted() {
    this.id = setInterval(() => {
      this.bottomTest();
      this.clearTest();
      this.down();
      this.moveTest();
    }, 500);
  },
  methods: {
    change(prev, next) {
      for (let y = 0; y < 20; y++) {
        for (let x = 0; x < 10; x++) {
          if (this.blocks[y][x] == prev) {
            this.blocks[y][x] = next;
          }
        }
      }
      this.blocks = [...this.blocks];
    },
    changeNext(prev, next) {
      for (let y = 0; y < 4; y++) {
        for (let x = 0; x < 4; x++) {
          if (this.next[y][x] == prev) {
            this.next[y][x] = next;
          }
        }
      }
      this.next = [...this.next];
    },
    left() {
      let canMove = true;
      for (let y = 0; y < 20; y++) {
        for (let x = 0; x < 10; x++) {
          if (this.blocks[y][x] == 1 && x == 0) {
            canMove = false;
          }
          if (this.blocks[y][x] == 1 && this.blocks[y][x - 1] == 2) {
            canMove = false;
          }
        }
      }
      if (!canMove) {
        return;
      }
      for (let y = 0; y < 20; y++) {
        for (let x = 0; x < 10; x++) {
          if (this.blocks[y][x] == 1) {
            this.blocks[y][x - 1] = 1;
            this.blocks[y][x] = 0;
          }
        }
      }
      this.origin[1]--;
      this.blocks = [...this.blocks];
    },
    right() {
      let canMove = true;
      for (let y = 0; y < 20; y++) {
        for (let x = 9; x >= 0; x--) {
          if (this.blocks[y][x] == 1 && x == 9) {
            canMove = false;
          }
          if (this.blocks[y][x] == 1 && this.blocks[y][x + 1] == 2) {
            canMove = false;
          }
        }
      }
      if (!canMove) {
        return;
      }
      for (let y = 0; y < 20; y++) {
        for (let x = 9; x >= 0; x--) {
          if (this.blocks[y][x] == 1) {
            this.blocks[y][x + 1] = 1;
            this.blocks[y][x] = 0;
          }
        }
      }
      this.origin[1]++;
      this.blocks = [...this.blocks];
    },
    down() {
      let canMove = true;
      for (let y = 19; y > 0; y--) {
        for (let x = 0; x < 10; x++) {
          if (this.blocks[y][x] == 1 && this.blocks[y + 1][x] == 2) {
            canMove = false;
          }
          if (this.blocks[y][x] == 1 && y >= 19) {
            canMove = false;
          }
        }
      }
      if (!canMove) {
        return;
      }
      for (let y = 19; y >= 0; y--) {
        for (let x = 0; x < 10; x++) {
          if (this.blocks[y][x] == 1) {
            this.blocks[y + 1][x] = 1;
            this.blocks[y][x] = 0;
          }
        }
      }
      this.origin[0]++;
      this.blocks = [...this.blocks];
    },
    getpos(type, angle, origin) {
      let shape = this.shape[type][angle];
      let pos = [];
      shape.forEach((row, y) => {
        row.forEach((block, x) => {
          if (block == 1) {
            pos.push([origin[0] + y, origin[1] + x]);
          }
        });
      });
      return pos;
    },
    show() {
      let pos = this.getpos(this.type, this.angle, this.origin);
      pos.forEach(pos => {
        this.blocks[pos[0]][pos[1]] = 1;
      });
      this.blocks = [...this.blocks];
    },
    showNext() {
      this.changeNext(1, 0);
      this.angle = 0;
      this.type = Math.floor(Math.random() * 7);
      let nextOrigin = [0, 0];
      let index = Math.floor(Math.random() * 8);
      this.randomColor = this.color[index];
      let pos = this.getpos(this.type, this.angle, nextOrigin);
      pos.forEach(pos => {
        this.next[pos[0]][pos[1]] = 1;
      });
      this.next = [...this.next];
    },
    add() {
      this.origin = [0, 3];
      this.angle = 0;
      this.type = Math.floor(Math.random() * 7);
      let canAdd = true;
      this.getpos(this.type, this.angle, this.origin).forEach(pos => {
        if (this.blocks[pos[0]][pos[1]] == 2) {
          canAdd = false;
        }
      });

      if (canAdd) {
        let index = Math.floor(Math.random() * 8);
        this.randomColor = this.color[index];
        this.show();
      } else {
        this.score = "GAME OVER";
        clearInterval(this.id);
      }
      //   this.showNext();
    },
    rotate() {
      let newAngle = (this.angle + 1) % 4;
      let canRotate = true;
      let pos = this.getpos(this.type, newAngle, this.origin);
      pos.forEach(pos => {
        if (pos[0] > 19) {
          canRotate = false;
        }
        if (pos[1] < 0 || pos[1] > 9) {
          canRotate = false;
        }
        if (this.blocks[pos[0]][pos[1]] == 2) {
          canRotate = false;
        }
      });
      if (!canRotate) {
        return;
      }
      this.angle = newAngle;
      this.change(1, 0);
      this.show();
    },
    control(event) {
      let keyCode = event.keyCode;
      switch (keyCode) {
        case 40:
          this.down();
          break;
        case 37:
          this.left();
          break;
        case 39:
          this.right();
          break;
        case 38:
          this.rotate();
          break;
        case 13:
          this.show();
          break;
        default:
          break;
      }
    },
    moveTest() {
      let haveActive = false;
      for (let y = 0; y < 20; y++) {
        for (let x = 0; x < 10; x++) {
          if (this.blocks[y][x] == 1) {
            haveActive = true;
          }
        }
      }

      if (!haveActive) {
        this.origin = [0, 3];
        this.add();
      }
    },
    bottomTest() {
      let isBottom = false;
      this.getpos(this.type, this.angle, this.origin).forEach(pos => {
        if (pos[0] >= 19) {
          isBottom = true;
        } else if (this.blocks[pos[0] + 1][pos[1]] == 2) {
          isBottom = true;
        }
      });
      if (isBottom) {
        this.change(1, 2);
      }
    },
    clearTest() {
      this.blocks.forEach((row, y) => {
        let canClear = row.every(block => {
          return block == 2;
        });
        if (canClear) {
          this.blocks.splice(y, 1);
          this.blocks.unshift([0, 0, 0, 0, 0, 0, 0, 0, 0, 0]);
          this.score = this.score + 10;
        }
      });
    },
    getColor(diamond) {
      if (diamond == 1) {
        return this.randomColor;
      } else if (diamond == 2) {
        return "#757575";
      } else {
        return "#fafafa";
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.tetris {
  position: relative;
  width: 100vw;
  height: 100vh;
  background-color: #e0e0e0;
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  .score {
    width: inherit;
    height: 10%;
    color: #e53935;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 2rem;
    // background-color: rgb(89, 163, 233);
    // margin: 5px;
  }
  .main {
    width: inherit;
    height: 70%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    // margin: 0.2rem;
    .row {
      width: 100%;
      height: 5%;
      display: flex;
      justify-content: center;
      align-items: center;
      box-sizing: border-box;
      margin: 0.1rem 0px;
      .diamond {
        width: 10%;
        height: 100%;
        background-color: #fafafa;
        box-sizing: border-box;
        margin:0.1rem;
        border-radius: 0.1rem;
      }
    }
  }
  .panel {
    position: relative;
    width: 100%;
    height: 20%;
    .icon {
      position: absolute;
      width: 3rem;
      height: 3rem;
      font-size: 1rem;
    }
    .down {
      bottom: 1rem;
      left: 5.2rem;
    }
    .left {
      top: 1rem;
      left: 2.3rem;
    }
    .right {
      top: 1rem;
      left: 8.3rem;
    }
    .rotate {
      width: 5rem;
      height: 5rem;
      right: 2.8rem;
      top: 1.5rem;
    }
  }
}
.move {
  background-color: #66bb6a !important;
}
.done {
  background-color: #757575 !important;
}
</style>