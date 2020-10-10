<template>
  <div>
    <div class="select">
      <el-select
        v-model="userValue"
        placeholder="选择用户"
        @change="userChange"
        :disabled="(btnBoolean == btnStatus)== 1 ? true : false"
      >
        <el-option
          v-for="item in userList"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        >
        </el-option>
      </el-select>
      <el-input
        type="text"
        v-model="code"
        placeholder="请输入内容"
        clearable
        onkeyup="this.value = this.value.replace(/[^\d.]/g,'');"
        maxlength="8"
        show-word-limit
        @input="codeChange"
        :disabled="(btnBoolean == btnStatus)== 1 ? true : false"
      ></el-input>
    </div >
    <div class = "codes">
      <div class="totalTime" v-show="numOpen">
       {{totalTime}}
      </div>
      <div  v-for="code in codeList" :key="code.index" class="code">{{code}}</div>
    </div>
    <div>
      <el-button class="play" @click="play" :disabled="(btnBoolean == btnStatus)== 1 ? true : false">播 放</el-button>
    </div>

  </div>
</template>

<script>
export default {
  data () {
    return {
      userList: [
        {
          value: 'yzg',
          label: '叶志刚'
        },
        {
          value: 'fhg',
          label: '方海根'
        },
        {
          value: 'zj',
          label: '张 健'
        },
        {
          value: 'xy',
          label: '徐 源'
        }
      ],
      soundList: [],
      codeList: [],
      userValue: 'yzg',
      code: '',
      filePath: '/static/voice/yzg/',
      count: 0,
      btnBoolean: Boolean,
      btnStatus: -1,
      totalTime: 5,
      numOpen: false
    }
  },
  methods: {
    userChange () {
      this.filePath = '/static/voice/' + this.userValue + '/'
    },
    codeChange () {
      const codeList1 = Array.from(this.code, (value) => value)
      if (codeList1.length > 4) {
        codeList1.splice(4, 0, '-')
      }
      this.codeList = codeList1
    },
    play () {
      this.btnBoolean = -1
      this.imgOpen = true
      this.countDown()
      this.listplay()
      this.count = 0
      const arr = this.soundList
      var myAudio = new Audio()
      myAudio.src = arr.shift() // 每次读数组最后一个元素
      myAudio.addEventListener('ended', playEndedHandler, false)
      myAudio.addEventListener('ended', count.bind(this), false)
      myAudio.play()
      myAudio.loop = false// 禁止循环，否则无法触发ended事件
      function playEndedHandler () {
        myAudio.src = arr.shift()
        myAudio.play()
        !arr.length && myAudio.removeEventListener('ended', playEndedHandler, false)// 只有一个元素时解除绑定
        !arr.length && myAudio.removeEventListener('ended', count.bind(this), false)
      }
      function count () {
        this.count++
        if (this.count > 1) {
          this.imgOpen = false
        }
        if (this.count === (this.code.length + 2)) {
          this.btnBoolean = 1
        }
      }
    },
    listplay () {
      const str = this.code
      const filePath = this.filePath
      const arr = Array.from(str, (value) => filePath + value + '.mp3')
      const arr1 = '/static/voice/5.mp3'
      const arr2 = '/static/voice/99.mp3'
      // 添加空声音99.mp3
      arr.unshift(arr1)
      arr.splice(5, 0, arr2)
      this.soundList = arr
    },
    countDown () {
      this.numOpen = true
      const clock = setInterval(() => {
        this.totalTime--
        if (this.totalTime < 1) {
          window.clearInterval(clock)
          this.totalTime = 5
          this.numOpen = false
        }
      }, 1000)
    }

  }
}

</script>

<style lang="scss" scoped>
.select {
  display: flex;
}
.codes{
  display: flex;
  margin-top: 20px;
  justify-content: center;
  height: 40px;
  font-size: 37px;
  align-items: center;
}
.code{
  color: #67c23a;
  height: 40px;
}
.totalTime{
  width: 40px !important;
  margin-right: 15px;
  color: red;
}
.play {
  display: flex;
  justify-content: center;
  width: 100% !important;
  margin-top: 20px;
}
</style>
