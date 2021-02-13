<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/peiko-logo-mini.png">
    <getStock ></getStock>
  </div>

</template>

<script>

import getStock from "./modals/get-stock"

export default {
  components: {getStock},
  name: "App",
  data() {
    return {
      no_data: false,
      clicked: false,
      preloader: false,
      data: []
    }
  },


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

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 30px;
}
</style>
