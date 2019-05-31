<template>
  <div id="app">
    <canvas id="clock" width="500" height="500"></canvas>
    <div class="control">
      <section>
        <button @click="hourBtn(true)">+</button>
        <button @click="minuteBtn(true)">+</button>
        <button @click="secondBtn(true)">+</button>
      </section>
      <section class="time">
        <span>{{ hour > 9 ? hour : '0' + hour }}</span>
        <span>:</span>
        <span>{{ minute > 9 ? minute : '0' + minute }}</span>
        <span>:</span>
        <span>{{ second > 9 ? second : '0' + second }}</span>
      </section>
      <section>
        <button @click="hourBtn(false)">-</button>
        <button @click="minuteBtn(false)">-</button>
        <button @click="secondBtn(false)">-</button>
      </section>
    </div>
  </div>
</template>
<script>
export default {
  mounted () {
    this.ctx = document.getElementById('clock').getContext('2d')
    this.draw(this.hour, this.minute, this.second)
    this.init()
  },
  data () {
    return {
      time: new Date(),
      ctx: null,
      hour_add: 0,
      minute_add: 0,
      second_add: 0,
      interval: null
    }
  },
  methods: {
    init () {
      this.interval = setInterval(() => {
        this.time = new Date()
      }, 1000)
    },
    draw (hour, minute, second) {
      let ctx = this.ctx
      ctx.clearRect(0, 0, 500, 500)
      ctx.beginPath()
      ctx.lineWidth = 20
      ctx.strokeStyle = 'rgb(50,95,162)'
      ctx.arc(250, 250, 200, 0, 360)
      ctx.stroke()
      ctx.closePath()
      for (let i = 0; i < 12; i++) {
        ctx.beginPath()
        ctx.save()
        ctx.lineCap = 'round'
        ctx.lineWidth = 7
        ctx.strokeStyle = '#000'
        ctx.translate(250, 250)
        ctx.rotate(i * 30 * Math.PI / 180)
        ctx.moveTo(0, -165)
        ctx.lineTo(0, -185)
        ctx.stroke()
        ctx.restore()
        ctx.closePath()
      }
      for (let i = 1; i <= 60; i++) {
        ctx.beginPath()
        ctx.save()
        ctx.lineCap = 'round'
        ctx.lineWidth = 4
        ctx.strokeStyle = '#000'
        ctx.translate(250, 250)
        ctx.rotate(i * 6 * Math.PI / 180)
        ctx.moveTo(0, -178)
        ctx.lineTo(0, -185)
        ctx.stroke()
        ctx.restore()
        ctx.closePath()
      }
      // 分针
      ctx.beginPath()
      ctx.save()
      ctx.lineCap = 'round'
      ctx.lineWidth = 7
      ctx.strokeStyle = '#000'
      ctx.translate(250, 250)
      ctx.rotate(minute * Math.PI / 30 + second * Math.PI / 1800)
      ctx.moveTo(0, 15)
      ctx.lineTo(0, -170)
      ctx.stroke()
      ctx.restore()
      ctx.closePath()
      // 时针
      ctx.beginPath()
      ctx.save()
      ctx.lineCap = 'round'
      ctx.lineWidth = 10
      ctx.strokeStyle = '#000'
      ctx.translate(250, 250)
      ctx.rotate(hour * Math.PI / 6 + minute * Math.PI / 360)
      ctx.moveTo(0, 15)
      ctx.lineTo(0, -140)
      ctx.stroke()
      ctx.restore()
      ctx.closePath()
      // 秒针
      ctx.beginPath()
      ctx.save()
      ctx.lineCap = 'round'
      ctx.lineWidth = 10
      ctx.strokeStyle = '#d00'
      ctx.translate(250, 250)
      ctx.rotate(second * Math.PI / 30)
      ctx.moveTo(0, 15)
      ctx.lineTo(0, -150)
      ctx.stroke()
      ctx.closePath()
      ctx.beginPath()
      ctx.strokeStyle = '#d00'
      ctx.arc(0, -160, 10, 0, 360)
      ctx.stroke()
      ctx.restore()
      ctx.closePath()
      // 交叉点
      ctx.beginPath()
      ctx.fillStyle = '#d00'
      ctx.arc(250, 250, 12, 0, 360)
      ctx.fill()
      ctx.closePath()
    },
    hourBtn (flag) {
      if (flag) {
        this.hour_add++
        if (this.hour === 24) {
          this.hour_add = -this.time.getHours()
        }
      } else {
        this.hour_add--
        if (this.hour < 0) {
          this.hour_add = 23 - this.time.getHours()
        }
      }
      this.draw(this.hour, this.minute, this.second)
    },
    minuteBtn (flag) {
      if (flag) {
        this.minute_add++
        if (this.minute === 60) {
          this.minute_add = -this.time.getMinutes()
          this.hour_add++
        }
      } else {
        this.minute_add--
        if (this.minute < 0) {
          this.minute_add = 59 - this.time.getMinutes()
          this.hour_add--
        }
      }
      this.draw(this.hour, this.minute, this.second)
    },
    secondBtn (flag) {
      if (flag) {
        this.second_add++
        if (this.second >= 60) {
          this.second_add = -this.time.getSeconds()
          this.minute_add++
        }
      } else {
        this.second_add--
        if (this.second < 0) {
          this.second_add = 59 - this.time.getSeconds()
          this.minute_add--
        }
      }
      this.draw(this.hour, this.minute, this.second)
    }
  },
  watch: {
    time () {
      this.draw(this.hour, this.minute, this.second)
    }
  },
  computed: {
    hour () {
      let hours = this.time.getHours() + this.hour_add
      return hours
    },
    minute () {
      return this.time.getMinutes() + this.minute_add
    },
    second () {
      return this.time.getSeconds() + this.second_add
    }
  }
}
</script>
<style lang="scss">
* {
  padding: 0;
  margin: 0;

}
:root {
  font-size: 1vw;
  display: flex;
  justify-content: center;
  align-items: center;
}
@media screen and(max-width : 800px){
  :root{
    font-size: 2vw;
  }
}

#app {
  #clock{
    width: 30rem;
    height: 30rem;
  }
  .control{
    width: 30rem;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    button{
      font-size: 3rem;
      width: 7rem;
      height: 3rem;
      line-height: 2rem;
      border-radius: 0.1rem;
      outline: none;
      background: #fff;
      border: 1px solid #333;
      border-radius: 0.5rem;
      margin: 0 0.3rem;
      &:active{
        background: rgba(0, 0, 0, 0.1);
      }
    }
    .time{
      font-size: 2rem;
      height: 3rem;
      line-height: 3rem;
      span:nth-of-type(2n+1){
        display: inline-block;
        width: 7rem;
        text-align: center;
      }
      span:nth-of-type(2n){
        display: inline-block;
        width: 0.3rem;
        text-align: center;
      }
    }
  }
}
</style>
