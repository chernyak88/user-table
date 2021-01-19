<template>
  <div id="app" class="app">
    <Table/>
  </div>
</template>

<script>
import Table from './components/Table'

export default {
  name: 'App',
  components: {
    Table
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}
.app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 90px;
}
/* когда приложение станет больше, стоит вынести button в отдельный компонент */
button {
  width: 150px;
  height: 30px;
  border-radius: 15px;
  cursor: pointer;
  outline: none;
}
button:hover {
  opacity: .6;
}
</style>
