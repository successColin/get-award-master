<template>
  <div>
    <h1 class="title">戴青云宝宝生日抽奖活动</h1>
    <div id="rotary-table">
      <div
        class="award"
        v-for="(award, index) in awards"
        :key="index"
        :class="['award' + index, { active: index == current }]"
      >
        {{ award.name }}
      </div>
      <div id="start-btn" @click="start">开始</div>
    </div>
    <div class="count">抽奖次数： {{ count }}/5</div>
    <div class="count">游戏说明：共5次抽奖机会，可从5次抽奖结果选择其一兑换奖品，本游戏一切解释权由陈明宽说明</div>
  </div>
</template>
<script>
export default {
  name: 'raffle',
  data() {
    return {
      current: 0,
      awards: [
        { id: 8, name: '金/银手镯' },
        { id: 2, name: '金手镯' },
        { id: 3, name: '银手镯' },
        { id: 1, name: '谢谢惠顾' },
        { id: 4, name: '8.2 元' },
        { id: 5, name: '82 元' },
        { id: 6, name: '820 元' },
        { id: 7, name: '8200 元' },
      ],
      speed: 200,
      diff: 15,
      award: {},
      time: 0,
      count: 5,
    };
  },
  methods: {
    start() {
        if(this.count <= 0){
            alert('您已经没有抽奖次数')
            return
        }
        
      // 开始抽奖
      this.drawAward();

      this.time = Date.now();
      this.speed = 200;
      this.diff = 15;
    },
    drawAward() {
      // 请求接口, 这里我就模拟请求后的数据(请求时间为2s)
      const random = Math.floor(Math.random()*7)
      if (this.count === 3 || this.count === 4 || this.count === 5) {
        setTimeout(() => {
          this.award = { id: 1, name: '谢谢惠顾' }
        }, 1000);
      } else if(this.count === 2){
        setTimeout(() => {
          this.award = this.awards[random]
        }, 1000);
      } else {
        setTimeout(() => {
          this.award = { id: 8, name: '金/银手镯' }
        }, 1000);
      }
      this.count--
      localStorage.count = this.count
      this.move();
    },
    move() {
      window.timeout = setTimeout(() => {
        this.current++;
        if (this.current > 7) {
          this.current = 0;
        }
        if (this.award.id && (Date.now() - this.time) / 1000 > 2) {
          this.speed += this.diff;
          if (
            (Date.now() - this.time) / 1000 > 4 &&
            this.award.id == this.awards[this.current].id
          ) {
            clearTimeout(window.timeout);
            setTimeout(() => {
              alert('恭喜你，抽中了' + this.award.name);
            }, 0);
            return;
          }
        } else {
          this.speed -= this.diff;
        }
        this.move();
      }, this.speed);
    },
  },
  mounted(){
    if(localStorage.count){
        this.count = localStorage.count
    }
  }
};
</script>

<style rel="stylesheet/less" lang="less">
* {
  margin: 0;
  padding: 0;
  list-style: none;
  outline: none;
}
.title {
  font-size: 30px;
  text-align: center;
  line-height: 40px;
  margin-bottom: 20px;
}
.count {
  text-align: center;
  margin-top: 20px;
}
#rotary-table {
  width: 340px;
  height: 349px;
  position: relative;
  margin: auto;
  background-color: antiquewhite;

  .award {
    width: 90px;
    height: 96px;
    line-height: 96px;
    text-align: center;
    float: left;
    position: absolute;
    overflow: hidden;
    background-color: aquamarine;

    &.active {
      background-color: darkgoldenrod;
    }

    &.award0 {
      top: 21px;
      left: 21px;
    }
    &.award1 {
      top: 21px;
      left: 125px;
    }
    &.award2 {
      top: 21px;
      right: 22px;
    }
    &.award3 {
      top: 126px;
      right: 22px;
    }
    &.award4 {
      bottom: 22.5px;
      right: 22px;
    }
    &.award5 {
      bottom: 22.5px;
      right: 125.5px;
    }
    &.award6 {
      bottom: 22.5px;
      left: 21px;
    }
    &.award7 {
      top: 126px;
      left: 21px;
    }
  }
  #start-btn {
    position: absolute;
    top: 125px;
    left: 124px;
    width: 90px;
    height: 96px;
    line-height: 90px;
    text-align: center;
    background-color: coral;
  }
}
</style>