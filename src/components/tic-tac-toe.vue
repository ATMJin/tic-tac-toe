<template>
  <div class="tic-tac-toe">
    <div class="row block" v-for="j in 3" :key="j">
      <div :class="['col', 'block', { 'oo': maru[i][j], 'xx': !maru[i][j], 'active': active[i][j] }]" v-for="i in 3"
        :key="[i, j]" :data-local="[i, j]" @click="tic($event)">
      </div>
    </div>
    <button @click="reset">reset</button>
    <div class="cover" :style="display ? `display:block` : `display:none`"></div>
  </div>
</template>

<script>
import { ref, reactive } from 'vue';
export default {
  setup(props) {
    // 圈圈，用兩個陣列表示位置，true是圈
    let maru = reactive([]);
    // 是否已被按，用兩個陣列表示位置，true是已經按了
    let active = reactive([]);
    // 最上層是否覆蓋，false是display:none
    let display = ref(true);
    // 下的次數
    let count = 0;
    // 輪到誰
    let turn = true;

    // 歸零
    const reset = () => {
      // 全部都設成圈且未按
      for (let i = 1; i < 4; i++) {
        maru[i] = [];
        active[i] = [];
        for (let j = 1; j < 4; j++) {
          maru[i][j] = true;
          active[i][j] = false;
        }
      }
      count = 0;
      display.value = false;
      turn = true;
    };
    reset();

    // 點擊事件
    const tic = (e) => {
      // 獲取位置
      let local = e.target.dataset.local.split(',');
      // 已被點過直接跳出函式
      if (active[local[0]][local[1]]) {
        return;
      }
      maru[local[0]][local[1]] = turn;
      active[local[0]][local[1]] = true;
      turn = !turn;
      count++;
      // 判斷輸贏
      if (count > 4) {
        whoWin();
        if (count == 9) {
          // 平手
          setTimeout(() => { alert("Over \nPlay again? \nPlease click reset button."); }, 50);
        }
      }
    };

    // 贏了回傳true
    const win = () => {
      if (active[1][1] == true &&
        active[1][1] == active[2][2] && active[2][2] == active[3][3] &&
        maru[1][1] == maru[2][2] && maru[2][2] == maru[3][3]) {
        console.log("1");
        return true;
      } else if (active[1][3] == true &&
        active[1][3] == active[2][2] && active[2][2] == active[3][1] &&
        maru[1][3] == maru[2][2] && maru[2][2] == maru[3][1]) {
        console.log("2");
        return true;
      }
      for (let i = 1; i < 4; i++) {
        if (active[i][1] == true &&
          active[i][1] == active[i][2] && active[i][2] == active[i][3] &&
          maru[i][1] == maru[i][2] && maru[i][2] == maru[i][3]) {
          console.log("3");
          return true;
        } else if (active[1][i] == true &&
          active[1][i] == active[2][i] && active[2][i] == active[3][i] &&
          maru[1][i] == maru[2][i] && maru[2][i] == maru[3][i]) {
          console.log("4");
          return true;
        }
      }
    };

    const whoWin = () => {
      if (win()) {
        display.value = true;
        setTimeout(() => { alert(!turn ? "OO win !!!" : "XX win !!!"); }, 10);
        return;
      }
    };


    return {
      maru,
      active,
      display,
      tic,
      reset,
    };
  },
};
</script>

<style lang="scss">
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-size: 0;
}

.tic-tac-toe {
  position: relative;
  width: 300px;
  margin: 50px auto;
  text-align: center;

  .col {
    display: inline-block;
    width: 100px;
    height: 100px;
    border: 1px solid #000;
  }

  .active {
    &.oo {
      &::before {
        content: "";
        position: relative;
        display: block;
        left: 20px;
        top: 20px;
        width: 60px;
        height: 60px;
        outline: 3px solid #f00;
        border-radius: 50%;
      }
    }

    &.xx {

      &::before,
      &::after {
        content: "";
        position: relative;
        display: block;
        width: 80px;
        height: 3px;
        background: #00f;
      }

      &::before {
        transform-origin: left;
        transform: translateX(22px) translateY(22px) rotate(45deg);
      }

      &::after {
        transform-origin: right;
        transform: translateX(-2px) translateY(22px) rotate(-45deg);
      }
    }
  }

  button {
    font-size: 16px;
    margin-top: 10px;
  }

  .cover {
    position: absolute;
    top: 0;
    left: 0;
    width: 300px;
    height: 300px;
    background: #0000;
  }
}
</style>