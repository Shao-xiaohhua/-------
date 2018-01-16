<template>
  <div class="hello">
    <div class="one">
      <div class="line" v-for="(item, index) in line" :key="index">
        <div class="line-d"
          v-for="(itema, indexa) in item.d"
          :key="indexa"
          @click="addZW({i: indexa, ii: index})">
          <span class="zw" v-if="itema.zw.show">{{itema.zw.type}}</span>
        </div>
        <div class="js"
          @click="move({indexz: index, indexj: indexj, item: itemj})"
          v-bind:class="{ move: itemj.move, 'stop': itemj.st }"
          v-for="(itemj, indexj) in item.js"
          :key="itemj.name"
          ref="abc">
            <span class="name" v-if="!itemj.st">{{itemj.name}}</span>
            <span class="name" v-if="itemj.st">'僵尸吃东西'</span>
          </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      line: [
        {
          name: 'line1',
          d: [
            {name: 'd1', zw: {show: false, type: '向日葵', lafe: 20}, jl: 0},
            {name: 'd1', zw: {show: false, type: '向日葵', lafe: 20}, jl: 0},
            {name: 'd1', zw: {show: false, type: '向日葵', lafe: 20}, jl: 0},
            {name: 'd1', zw: {show: true, type: '向日葵', lafe: 20}, jl: 0},
            {name: 'd1', zw: {show: false, type: '向日葵', lafe: 20}, jl: 0},
            {name: 'd1', zw: {show: false, type: '向日葵', lafe: 20}, jl: 0},
            {name: 'd1', zw: {show: false, type: '向日葵', lafe: 20}, jl: 0},
            {name: 'd1', zw: {show: false, type: '向日葵', lafe: 20}, jl: 0},
            {name: 'd1', zw: {show: false, type: '向日葵', lafe: 20}, jl: 0}
          ],
          js: [
            {name: '僵尸1', style: {left: 0}, move: false, st: false},
            {name: '僵尸2', style: {left: 0}, move: false, st: false}
          ],
          move: 1200,
          index: 0
        },
        {
          name: 'line2',
          d: [
            {name: 'd1', zw: {show: false, type: '向日葵', lafe: 20}, jl: 0},
            {name: 'd1', zw: {show: false, type: '向日葵', lafe: 20}, jl: 0},
            {name: 'd1', zw: {show: false, type: '向日葵', lafe: 20}, jl: 0},
            {name: 'd1', zw: {show: false, type: '向日葵', lafe: 20}, jl: 0},
            {name: 'd1', zw: {show: false, type: '向日葵', lafe: 20}, jl: 0},
            {name: 'd1', zw: {show: true, type: '向日葵', lafe: 20}, jl: 0},
            {name: 'd1', zw: {show: false, type: '向日葵', lafe: 20}, jl: 0},
            {name: 'd1', zw: {show: false, type: '向日葵', lafe: 20}, jl: 0},
            {name: 'd1', zw: {show: false, type: '向日葵', lafe: 20}, jl: 0}
          ],
          js: [
            {name: '僵尸3', style: {left: 0}, move: false, st: false}
          ],
          move: 1200,
          index: 0
        }
      ]
    }
  },
  methods: {
    move (ind) {
      let ld = ind.indexz
      let lld = ind.indexj
      console.log(ind)
      this.line[ld].js[lld].move = true
      let it = ind.item
      this.csit(ld, lld, it)
    },
    csit (ld, lld, item) {
      let arr = item
      let num = arr.style.left
      let t = setInterval(() => {
        this.xh()
        num += 13
        let stm = this.line[ld].move
        if (num >= stm) {
          num += 0
          this.line[ld].js[lld].st = true
          this.eate(ld, lld, item)
          clearInterval(t)
        }
        this.line[ld].js[lld].style.left = num
      }, 1000)
      console.log(t)
    },
    eate (ld, lld, item) {
      console.log('正在吃哦')
      let num = this.line[ld].d[this.line[ld].index].zw.lafe
      let t
      console.log(t)
      let len = this.line[ld].js.length
      let jdn = 0
      for (let i = 0; i < len; i++) {
        if (this.line[ld].js[i].move === true) {
          jdn += 1
        }
      }
      t = setInterval(() => {
        num -= jdn
        this.line[ld].d[this.line[ld].index].zw.lafe = num
        if (this.line[ld].d[this.line[ld].index].zw.lafe <= 0) {
          this.line[ld].d[this.line[ld].index].zw.show = false
          this.line[ld].js[lld].st = false
          clearInterval(t)
          this.csit(ld, lld, item)
        }
        console.log('植物的血' + this.line[ld].d[this.line[ld].index].zw.lafe)
      }, 1000)
    },
    xh () {
      let len = this.line.length
      for (let i = 0; i < len; i++) {
        let lend = this.line[i].d.length
        let num = 1200
        let xb = 0
        for (let j = 0; j < lend; j++) {
          if (this.line[i].d[j].zw.show === true) {
            num = this.line[i].d[j].jl
            xb = j
            break
          }
        }
        this.line[i].move = parseInt(num)
        this.line[i].index = xb
        console.log(this.line[i].move)
      }
    },
    addZW (item) {
      let index = item.i
      let iindex = item.ii
      this.line[iindex].d[index].zw.show = true
    }
  },
  watch: {
    line () {
      console.log(this.line)
    }
  },
  created () {
    this.$nextTick(() => {
      let len = this.line.length
      for (let i = 0; i < len; i++) {
        let num = 1200 / 9
        let len = this.line[i].d.length
        for (let j = 0; j < len; j++) {
          this.line[i].d[j].jl = num * j
        }
      }
      this.$refs.abc[0].style.color = 'yellow'
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
.hello {
  width: 100%;
  height: 100%;
  .one {
    width: 1200px;
    height: 80%;
    margin: 50px auto 0 auto;
    background: black;
  }
  .line {
    position: relative;
    width: 100%;
    height: calc(100%/5);
    background: red;
  }
  .line-d {
    width: calc(100%/9);
    height: 100%;
    float: left;
    border: 1px solid black;
    box-sizing: border-box;
    background: green;
  }
  .js {
    position: absolute;
    top: 0;
    left: -80px;
    width: 80px;
    height: 100%;
    background: red;
    &.move {
      animation: move 92s linear;
    }
    &.stop {
      animation-play-state:paused;
      -webkit-animation-play-state:paused
    }
  }
}
@keyframes move {
  form {
  }
  to {
    transform: translate3D(1200px, 0, 0);
  }
}
</style>
