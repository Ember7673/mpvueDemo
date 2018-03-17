<template>
  <div>
    <div class="userinfo" @click="bindViewTap">
      <img class="userinfo-avatar" v-if="userInfo.avatarUrl" :src="userInfo.avatarUrl" background-size="cover" />
    </div>
    <div class="content-body flex">
        <div class="num inline-block">
          <transition name="num_move">
            <span>{{num1}}</span>
          </transition>
        </div>
        <div class="sign inline-block">+</div>
        <div class="num inline-block">
          <span>{{num2}}</span>
        </div>
        <div class="sign inline-block">=</div>
        <div class="num inline-block">
          <em>{{num3}}</em>
        </div>
    </div>
    <div class="answer">
        <ul>
          <li v-for="(item,i) in numList" :key="i" @click="isActive(i)">
            {{item}}
          </li>
        </ul>
    </div>
    <p>总分：{{count}}</p>
    <p>倒计时：{{time}}</p>
    <p v-show="isEnd">游戏结束</p>
    <button @click="startGame" v-show="isStart">游戏开始</button>
  </div>
</template>

<script>
import card from '@/components/card'

export default {
  data () {
    return {
      motto: 'Hello World',
      userInfo: {},
      num1: '',
      num2: '',
      num3: '',
      dstIndex: '',
      numList: [0,0,0,0,0,0,0,0,0],
      count: 0,
      time: 59,
      isEnd: false,
      isStart: false,
      timer1: null,
      timer2: null,
    }
  },

  components: {
    card
  },

  methods: {
    bindViewTap () {
      const url = '../logs/logs'
      wx.navigateTo({ url })
    },
    getUserInfo () {
      this.isStart = true
      // 调用登录接口
      wx.login({
        success: () => {
          wx.getUserInfo({
            success: (res) => {
              this.userInfo = res.userInfo
            }
          })
        }
      })
      // this.setRandomFn()
    },
    startGame() {
      this.setRandomFn()
      this.isStart = false
      this.isEnd = false
      this.count = 0
      clearInterval(this.timer2)
      this.time = 59
      this.timer2 = setInterval(() => {
        this.time--;
        if(this.time === 0) {
          this.isEnd = true
          this.gameOver()
        }
      }, 1000)
    },
    gameOver() {
      if(this.time === 0 && this.isEnd === true ) {
          clearInterval(this.timer1)
          clearInterval(this.timer2)
          this.num1 = ''
          this.num2 = ''
          this.num3 = ''
          // this.count = 0
          this.isEnd = true
          this.isStart = true
      }
    },
    deBugNum() {
      this.num1 = this.getRandomNum()
      this.num2 = this.getRandomNum()
    },
    getRandomNum () {
      return  Math.floor(Math.random() * 10);
    },
    setRandomFn() {
      clearInterval(this.timer1)
      this.timer1 = setInterval(() => {
        this.deBugNum()
        this.createdNumList()
      },3000)
    },
    createdNumList() {
      this.numList = [];
      for( var i = 0; i < 9; i++ ) {
        let num;
        var number = this.getRandomNum(num)
        this.numList.push(number)
      }
    },
    isActive(i) {
       this.num3 = this.numList[i]
       this.isSuccess()
    },
    isSuccess() {
      var number = this.num1 + this.num2
      if(number >= 10){
        number -= 10;
      }
      if(number === this.num3) {
        this.count ++ ;
      }
    },
    
  },

  created () {
    // 调用应用实例的方法获取全局数据
    this.getUserInfo()
    this.getRandomNum()
  }
}
</script>

<style scoped>
.userinfo {
  height: 128rpx;
  display: flex;
  /* flex-direction: column; */
  align-items: top;
  justify-content: center;
}

.userinfo-avatar {
  width: 128rpx;
  height: 128rpx;
  margin: 5rpx;
  border-radius: 50%;
  animation: mymove 1s forwards;
}
@keyframes mymove {
  from {
    width: 128rpx;
    height: 128rpx;
  }
  to {
    width: 70rpx;
    height: 70rpx;
  }
}

.content-body {
  width: 100%;
  height: 180rpx;
  margin-bottom: 30rpx;
}
.num {
  width: 180rpx;
  height: 180rpx;
  background-color: #000;
  color: #fff;
  text-align: center;
  line-height: 180rpx;
}
.num em {
  line-height: 180rpx;
}
/* .randomMove {
  animation: numMove 3s infinite;
} */
.num_move-enter-active, .num_move-leave-active {
  line-height: 370rpx;
    opacity: 0;
}
.num_move-enter, .num_move-leave-to {
  line-height: 0rpx;
  opacity: 0;
}
/* @keyframes numMove {
  from {
    line-height: 370rpx;
    opacity: 0;
  }
  50% {
    line-height: 180rpx;
    opacity: 1;
  }
  to {
    line-height: 0rpx;
    opacity: 0;
  }
} */
.sign {
  height: 180rpx;
  line-height: 180rpx;
}
.answer {
  width: 100%;
  /* height: 220rpx; */
  
}
.answer ul {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
}
.answer ul li {
  width: 33%;
  height: 200rpx;
  text-align: center;
  background-color: #000;
  color: #fff;
  line-height: 200rpx;
}
.answer ul li:nth-child(2n) {
  background-color: #fff;
  color: #000;
}
.answer ul li:hover {
  box-shadow: 10px 10px 10px gray inset;
}

</style>
