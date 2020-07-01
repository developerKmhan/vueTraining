<template>
  <div>
    <div>당첨 숫자</div>
    <div id="결과창">
      <lotto-ball v-for="ball in winBalls" :key="ball" v-bind:number="ball"></lotto-ball>
    </div>
    <div>보너스</div>
    <lotto-ball v-if="bonus" :number="bonus"></lotto-ball>
    <button v-if="redo" @click="onClickRedo">한 번 더!</button>
  </div>
</template>

<script>
  import LottoBall from './LottoBall';
  function getWinNumbers() {
    const candidate = Array(45).fill().map((v, i) => i + 1);
    const shuffle = [];
    while (candidate.length > 0) {
      shuffle.push(candidate.splice(Math.floor(Math.random() * candidate.length), 1)[0]);
    }
    const bonusNumber = shuffle[shuffle.length -1];
    const winNumbers = shuffle.slice(0, 6).sort((p, c) => p - c);
    return [...winNumbers, bonusNumber];
  }

  const timeouts = [];
  export default {
    components: {
      LottoBall,
    },
    data() {
      return {
        winNumbers: getWinNumbers(),
        winBalls: [],
        bonus: null,
        redo: false,
      };
    },
    computed: {
    },
    methods: {
      onClickRedo() {
        this.winNumbers = getWinNumbers();
        this.winBalls = [];
        this.bonus = null;
        this.redo = false;
        //this.showBalls();
      },
      showBalls() {
        for (let i=0; i < this.winNumbers.length - 1; i++) {
          timeouts[i] = setTimeout(() => {
            this.winBalls.push(this.winNumbers[i]);
          }, (i + 1) * 1000);
        }
        timeouts[6] = setTimeout(() => {
          this.bonus = this.winNumbers[6];
          this.redo = true;
        },7000);
      }
    },
    mounted() {
      this.showBalls();
    },
    beforeDestroy() {
      timeouts.forEach((t) => {
        clearTimeout(t);
      })
    },
    watch: {
      // 배열(객체)의 경우에 val과 oldVal이 동일한 문제가 발생하니 다른 변수를 쓸 것
      // 비동기나 여러개의 watch 사용으로 인해 에러가 발생할 수 있으니까 최대한 watch를 쓰지 말 것
      bonus(val, oldVal) { 
        if (val == null) {
          this.showBalls();
          console.log(val, oldVal);
        }
      }
    },
  }
</script>

<style>
  .ball {
    display: inline-block;
    border: 1px solid black;
    border-radius: 20px;
    width: 40px;
    height: 40px;
    line-height: 40px;
    font-size: 20px;
    text-align: center;
    margin-right: 20px;
  }
</style>