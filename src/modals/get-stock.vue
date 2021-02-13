<template>
  <div class="main">
    <div class="get_data" @click="getData" :class="[ clicked ? 'clicked' : '']">Get data</div>
    <template v-if="clicked">
      <div class="loading">...loading</div>
    </template>
    <template v-else>
      <div v-if="data.length" class="show_data">
        <div class="table_head">
          <div>Stock</div>
          <div>Current</div>
          <div>Change</div>
        </div>
        <div class="content_row" v-for="item in data" :key="item.stock">
          <div>{{ item.stock }}</div>
          <div>{{ item.current.toFixed(2) }}</div>
          <div :class="[ item.diff < 0 ? 'low_stock' : 'high_stock']">
            {{ (+item.diff > 0) ? "+" + item.diff.toFixed(2) : item.diff.toFixed(2) }}
          </div>
        </div>
      </div>
      <div v-else-if="no_data">no data</div>
    </template>
  </div>
</template>

<script>

import simulateAsyncReq from "../plugins/getDataFunc"
import payload from "../mocData/index.js"


export default {
  components: [simulateAsyncReq, payload],
  data() {
    return {
      no_data: false,
      clicked: false,
      preloader: false,
      data: []
    }
  },
  methods: {
    getData() {
      if(this.clicked) return
      this.no_data = false
      this.clicked = true
      this.data = []
      simulateAsyncReq(payload)
        .then((response) => {
          this.data = response
          this.data = response.stocks.map((item,i) => {
            return {
              stock: item,
              current: response.current[i],
              start: response.start[i],
              diff: (response.current[i] - response.start[i])

            }

          })
          this.data.sort(function (a,b) {
            return a.stock > b.stock ? 1 : -1
          })

        })
        .catch (() => {this.no_data = true})
        .finally(()=> {this.clicked = false})
    }
  }
}
</script>

<style lang="scss" scoped>
div {
  box-sizing:border-box;
}

.main {
  height: 394px;
  width: 295px;
  margin:  0 auto;
}

.show_data {
  width: 100%;
  border: 1px solid #E5E5E5;
  div {
    text-align: center;
    padding-left: 5px;
    padding-right: 5px;
  }
  .table_head {
    width: 100%;
    display: flex;
    font-weight: bold;
    justify-content: space-between;
    text-align: center;
    background-color: lavender;
    height: 35px;
    line-height: 35px;
    border-bottom: 1px solid #E5E5E5;
    div {
      flex: 1;
      &:first-child {
        text-align: left;
        padding-left: 10px;
      }
      &:last-child {
        text-align: right;
        padding-right: 10px;
      }
    }
  }

  .content_row {
    display: flex;
    justify-content: space-between;
    &:not(:last-child) {
      border-bottom: 1px solid #E5E5E5;
    }
    div {
      flex: 1;
      text-align: center;
      height: 40px;
      line-height: 40px;
      &:first-child {
        text-align: left;
        padding-left: 10px;
      }
      &:last-child {
        text-align: right;
        padding-right: 10px;
      }
    }
  }
}

.clicked {
  transform: scale(0.95);
  border: 1px solid grey;
}

.get_data {
  background-color: lavender;
  border: 1px solid #E5E5E5;
  width: 150px;
  height: 30px;
  line-height: 30px;
  border-radius: 3px;
  margin: 20px auto;
  cursor: pointer;
}

.low_stock {
  color: red;
}
.high_stock {
  color: green;
}
</style>
