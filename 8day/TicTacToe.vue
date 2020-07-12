<template>
  <div>
    <div>{{turn}}님의 턴입니다.</div>
    <table-component :table-data="tableData"/>
    <div v-if="winner">{{winner}}님의 승리!</div>
  </div>
</template>

<script>
  import TableComponent from './TableComponent';

  export default {
    components: {
      TableComponent,
    },
    data() {
      return {
        tableData: [
         ['','',''],
         ['','',''],
         ['','',''],
        ],
        turn: 'O',
        winner: '',
      };
    },
    methods: {
      onChangeData() {
        //this.turn = 'X'; // 기본은 반영됨
        //this.tableData[0][1] = 'O'; // 객체나 배열은 바로 반영안됨
        //Vue.set(this.tableData[1], 0, 'X');
        this.$set(this.tableData[1], 0, 'X'); // Vue.set과 동일
      }
    },
  };
</script>

<style>
  table {
    border-collapse: collapse;
  }
  td {
    border: 1px solid black;
    width: 40px;
    height: 40px;
    text-align: center;
  }
</style>