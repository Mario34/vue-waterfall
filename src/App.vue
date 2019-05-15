<template>
  <div class="box">
    <div class="col" ref="col1">
      <transition-group name="list">
        <div class="item" v-for="item in dataList1" :key="item.id"><div>{{item.id}}</div>{{item.text}}</div>
      </transition-group>
    </div>
    <div class="col" ref="col2">
      <transition-group name="list">
        <div class="item" v-for="item in dataList2" :key="item.id"><div>{{item.id}}</div>{{item.text}}</div>
      </transition-group>
    </div>
    <div class="col" ref="col3">
      <transition-group name="list">
        <div class="item" v-for="item in dataList3" :key="item.id"><div>{{item.id}}</div>{{item.text}}</div>
      </transition-group>
    </div>
    <div class="col" ref="col4">
      <transition-group name="list">
        <div class="item" v-for="item in dataList4" :key="item.id"><div>{{item.id}}</div>{{item.text}}</div>
      </transition-group>
    </div>
  </div>
</template>
<script>
import mockData from './assets/data'
export default {
  data() {
    return {
      mainMenuList: [],
      dataList1: [],
      dataList2: [],
      dataList3: [],
      dataList4: [],
    }
  },
  mounted() {
    this.fetchData().then((mockData) => {
      this.mainMenuList = mockData
      this.mountMenu()
    })
  },
  methods: {
    fetchData() {
      /** 模拟数据请求*/
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve(mockData)
        }, 300)
      })
    },
    mountMenu(arg) {
      var temp = this.mainMenuList
      var index = arg || 0
      var refName = this.selectCol()
      if (temp.length > index) {
        this[refName].push(this.mainMenuList[index])
        this.$nextTick(() => {
          this.mountMenu(index + 1)
        })
      }
    },
    selectCol() {
      var getHeight = (ref) => {
        return this.$refs[ref].offsetHeight
      }
      var height1 = getHeight('col1')
      var height2 = getHeight('col2')
      var height3 = getHeight('col3')
      var height4 = getHeight('col4')
      switch (Math.min(height1, height2, height3, height4)) {
        case height1:
          return 'dataList1'
          break
        case height2:
          return 'dataList2'
          break
        case height3:
          return 'dataList3'
        case height4:
          return 'dataList4'
          break
      }
    },
  }
}
</script>
<style scoped>
/* transition样式 */
.list-enter-active,
.list-leave-active {
  transition: all 1s;
  overflow: hidden;
}
.list-enter,
.list-leave-to {
  opacity: 0;
  transform: translateY(60px);
}

.box {
  width: 1200px;
  min-height: 10px;
  margin: 0 auto;
  border: 1px solid #777;
  overflow: hidden;
  padding: 20px;
}
.col {
  width: 280px;
  float: left;
  margin-right: 20px;
}
.item {
  border: 1px solid #999;
  margin-bottom: 20px;
}
.col:last-child {
  margin-right: 0;
}
</style>

