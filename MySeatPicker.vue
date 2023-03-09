<template>
  <div id="mySeatPicker">
    <div class="tips" v-show="showTips">
      <button class="checked seat seatTip"/>:已被选择
      <button class="broken seat seatTip"/>:检修中
      <button class="beta seat seatTip"/>:按摩椅
      <button class="selected seat seatTip"/>:选中
    </div>

    <div v-for="rowIndex in rows" :key="'row_'+rowIndex" class="row">
      <div v-for="columnIndex in columns" :key="'role_'+rowIndex+'_column_'+columnIndex" class="col">
        <button
            class="seat"
            :class="{
                  broken:broken.includes(calcSeatNum(rowIndex,columnIndex)),
                  checked: checked.includes(calcSeatNum(rowIndex,columnIndex)),
                  selected: selected.includes(calcSeatNum(rowIndex,columnIndex)),
                  beta: beta.includes(calcSeatNum(rowIndex,columnIndex)),
                  none: !validSeatNum.includes(calcSeatNum(rowIndex,columnIndex))
              }"
            @click="doSelect(rowIndex,columnIndex)"
        >
<!--          {{ calcSeatNum(rowIndex, columnIndex) }}-->
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "MySeatPicker",
  created() {
  },
  props: {
    rows: {
      type: Number,
      default: 8
    },
    columns: {
      type: Number,
      default: 8
    },
    validSeatNum: {
      type: Array,
      default: () =>
          [1, 2, 3, 4, 5, 6, 7, 8,
            9, 10, 11, 12, 14, 15, 16,
            17, 18, 19, 20, 21, 22, 23, 24,
            25, 27, 28, 29, 30, 32, 33, 34, 35, 36,
            37, 38, 39, 40,
            41, 43, 44, 45, 46, 47, 48,
            49, 51, 52, 53, 54, 55, 56,
            57, 58, 59, 60, 61, 62, 63, 64
          ]
    },
    checked: {
      type: Array,
      default: () => [1, 10]
    },
    beta: {
      type: Array,
      default: () => [13, 15]
    },
    broken: {
      type: Array,
      default: () => [6, 12, 64]
    },
    showTips:{
      type:Boolean,
      default:true
    }
  },
  data() {
    return {
      selected: []
    }
  },
  methods: {
    calcSeatNum(row, col) {
      return (row - 1) * this.columns + col
    },
    doSelect(rowIndex, ColumnIndex) {
      let seatNum = this.calcSeatNum(rowIndex, ColumnIndex)
      if (this.selected.includes(seatNum)) {
        this.selected = this.selected.filter(item => item !== seatNum)
      } else {
        this.selected.push(seatNum)
      }
      console.log(`选中${rowIndex}排${ColumnIndex}座--座位编号:${seatNum}`)
      console.log('当前选中列表', this.selected)
    }
  }
}
</script>

<style scoped>
#mySeatPicker {
  --checked-color: red;
  --selected-color: green;
  --broken-color: #a59d9d;
  font-size: 0.9em;
}

.row{
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  gap: 4px;
}

.col {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.seat{
  cursor: pointer;
  width: 25px;
  height: 25px;
  border: 1px solid #a19c9c;
}


.seat.broken {
  cursor: not-allowed;
  background-color: var(--broken-color);
  pointer-events: none;
}

.seat.beta {
  border-radius: 50%;
  border-color: deeppink;
}

.seat.checked {
  background-color: var(--checked-color);
  cursor: not-allowed;
  pointer-events: none;
}

.seat.selected {
  background-color: var(--selected-color);
}

.seat.none {
  opacity: 0;
  cursor: not-allowed;
  pointer-events: none;
}

.seatTip{
  width: 10px;
  height: 10px;
  margin-bottom: 10px;
}




</style>