<template>
  <div class="byted-weektime">

    <div class="calendar">
      <table class="calendar-table" style="width:730px">
        <thead class="calendar-head">
        <tr>
          <th rowspan="6" colspan="6" class="week-td">星期/时间</th>
          <th colspan="24">00:00 - 12:00</th>
          <th colspan="24">12:00 - 24:00</th>
        </tr>
        <tr>
          <td colspan="2" v-for="index in tableHeader" :key="index">{{index}}</td>
        </tr>
        </thead>
        <tbody id="tableBody">
        <div
          id="kuang"
          :style="{width:kuangObj.width+'px',height:kuangObj.height+'px',top:kuangObj.top+'px',left:kuangObj.left+'px',bottom:kuangObj.bottom+'px',right:kuangObj.right+'px'}"
        ></div>
        <tr>
          <td colspan="6">星期一</td>
          <td
            @mousedown.prevent="handleMouseDown(i,0)"
            @mouseup.prevent="handleMouseUp(i,0)"
            class="calendar-atom-time"
            :class="item.class"
            v-for="(item,i) in rowUnit[0]"
            :key="i"
          ></td>
        </tr>
        <tr>
          <td colspan="6">星期二</td>
          <td

            @mousedown.prevent="handleMouseDown(i,1)"
            @mouseup.prevent="handleMouseUp(i,1)"
            class="calendar-atom-time"
            :class="item.class"
            v-for="(item,i) in rowUnit[1]"
            :key="i"
          ></td>
        </tr>
        <tr>
          <td colspan="6">星期三</td>
          <td

            @mousedown.prevent="handleMouseDown(i,2)"
            @mouseup.prevent="handleMouseUp(i,2)"
            class="calendar-atom-time"
            :class="item.class"
            v-for="(item,i) in rowUnit[2]"
            :key="i"
          ></td>
        </tr>
        <tr>
          <td colspan="6">星期四</td>
          <td

            @mousedown.prevent="handleMouseDown(i,3)"
            @mouseup.prevent="handleMouseUp(i,3)"
            class="calendar-atom-time"
            :class="item.class"
            v-for="(item,i) in rowUnit[3]"
            :key="i"
          ></td>
        </tr>
        <tr>
          <td colspan="6">星期五</td>
          <td
            @mousedown.prevent="handleMouseDown(i,4)"
            @mouseup.prevent="handleMouseUp(i,4)"
            class="calendar-atom-time"
            :class="item.class"
            v-for="(item,i) in rowUnit[4]"
            :key="i"
          ></td>
        </tr>
        <tr>
          <td colspan="6">星期六</td>
          <td

            @mousedown.prevent="handleMouseDown(i,5)"
            @mouseup.prevent="handleMouseUp(i,5)"
            class="calendar-atom-time"
            :class="item.class"
            v-for="(item,i) in rowUnit[5]"
            :key="i"
          ></td>
        </tr>
        <tr>
          <td colspan="6">星期日</td>
          <td

            @mousedown.prevent="handleMouseDown(i,6)"
            @mouseup.prevent="handleMouseUp(i,6)"
            class="calendar-atom-time"
            :class="item.class"
            v-for="(item,i) in rowUnit[6]"
            :key="i"
          ></td>
        </tr>

        <tr>
          <td  style="width: 100%;" colspan="55" class="grid-content1">
            <el-row type="flex" class="row-bg" justify="space-between">
              <el-col :span="5">
                <div>
                  <span style="line-height:40px;">{{isSelectSchedule == true ? '已选择时间段':'可拖动鼠标选择时间段'}}</span>
                </div>
                </el-col>
              <el-col  :span="2"><div>
                      <el-button type="text" @click="clear" class="pull-right">清空</el-button>
                </div></el-col>
            </el-row>
          </td>
        </tr>
        <tr>
          <td colspan="55" class="timeContent">
            <div v-for="(item,index) in timeSchedule"  style="text-align:left;margin-top:10px;" :key="index" v-show="item.length">
             <span>&nbsp;&nbsp;{{weekDate[index+1]}}:</span>
             <span>{{item | formatItem}}</span>
            </div>
          </td>
        </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MyCalendar',
  props: {
    value: String
  },
  data () {
    return {
      tableHeader: ['00', '01', '02', '03', '04', '05', '06', '07', '08', '09', '10', '11', '12', '13', '14', '15', '16', '17', '18', '19', '20', '21', '22', '23'],
      weekDate: {'1': '星期一', '2': '星期二', '3': '星期三', '4': '星期四', '5': '星期五', '6': '星期六', '7': '星期日'},
      rowUnit: [],
      num: [],
      beginDay: 0,
      beginTime: 0,
      downEvent: false,
      kuangObj: {
        width: 0,
        height: 0,
        top: 0,
        left: 0,
        bottom: 0,
        right: 0,
        oldLeft: 0,
        oldTop: 0,
        flag: false
      },
      timeTitle: [],
      timeSchedule: [],
      isSelectSchedule: false
    }
  },
  created () {
    this.init()
  },
  mounted () {
    let oBox = document.getElementById('tableBody')
    let tab = document.getElementsByClassName('calendar')[0]
    let oDiv = document.getElementById('kuang')
    oBox.onmousedown = function (ev) {
      var x1 = ev.clientX - tab.offsetLeft
      var y1 = ev.pageY - tab.offsetTop
      oBox.onmousemove = function (ev) {
        var x2 = ev.clientX - tab.offsetLeft
        var y2 = ev.pageY - tab.offsetTop
        oDiv.style.left = (x2 > x1 ? x1 : x2 + 2) + 'px'
        oDiv.style.top = (y2 > y1 ? y1 : y2 + 2) + 'px'
        oDiv.style.width = Math.abs(x2 - x1) + 'px'
        oDiv.style.height = Math.abs(y2 - y1) + 'px'
      }
      return false
    }
    document.onmouseup = function () {
      oBox.onmousemove = null
      oDiv.style.left = 0 + 'px'
      oDiv.style.top = 0 + 'px'
      oDiv.style.width = 0 + 'px'
      oDiv.style.height = 0 + 'px'
    }
  },
  methods: {
    init () {
      if (this.value != null && this.value !== '') {
        var list = this.value.split('')
        var templist = []
        var tempArr = []
        for (let i = 0; i < list.length; i++) {
          if (i % 48 === 0) {
            templist = []
            tempArr = []
            this.num.push(templist)
            this.rowUnit.push(tempArr)
          }
          var data = list[i]
          var dataNum = parseInt(data)
          tempArr.push({class: dataNum === 1 ? 'ui-selected' : null, timeData: dataNum})
          templist.push(dataNum)
        }
        console.log(this.num)
      } else {
        for (let i = 0; i < 7; i++) {
          let arr = []
          for (let j = 0; j < 48; j++) {
            arr.push({class: null, timeData: j})
          }
          this.rowUnit.push(arr)
        }
        for (let i = 0; i < 7; i++) {
          let arr1 = []
          for (let j = 0; j < 48; j++) {
            arr1.push(0)
          }
          this.num.push(arr1)
        }
      }
      for (let i = 0; i < this.tableHeader.length; i++) {
        this.timeTitle.push(this.tableHeader[i])
        this.timeTitle.push(this.tableHeader[i])
      }
       this.timeTitle.push('24')

    },
    handleMouseDown (i, day) {
      this.downEvent = true
      this.beginDay = day
      this.beginTime = i
    },
    handleMouseUp (i, day) {
      if (this.downEvent) {
        let _this = this
        let begin = this.beginTime
        let start = begin <= i ? begin : i
        let length = Math.abs(begin - i)
        let end = start + length
        let dayStart = this.beginDay <= day ? this.beginDay : day
        let dayLength = Math.abs(this.beginDay - day)
        let dayEnd = dayStart + dayLength

        // eslint-disable-next-line no-inner-declarations
        function isAdd () {
          for (let x = dayStart; x < dayEnd + 1; x++) {
            for (let y = start; y < end + 1; y++) {
              if (_this.rowUnit[x][y].class == null) return true
            }
          }
          return false
        }

        if (isAdd()) {
          for (let x = dayStart; x < dayEnd + 1; x++) {
            for (let y = start; y < end + 1; y++) {
              if (this.rowUnit[x][y].class == null) {
                this.rowUnit[x][y].class = 'ui-selected'
                this.num[x][y] = 1
              }
            }
          }
        } else {
          for (let x = dayStart; x < dayEnd + 1; x++) {
            for (let y = start; y < end + 1; y++) {
              this.rowUnit[x][y].class = null
              this.num[x][y] = 0
            }
          }
        }
      }
      this.changePropsValue()
      this.downEvent = false
      this.showSchedule()
    },
    clear () {
      this.rowUnit.forEach((item) => {
        item.forEach((item1) => {
          item1.class = null
        })
      })
      for (let i = 0; i < 7; i++) {
        for (let j = 0; j < 48; j++) {
          this.num[i][j] = 0
        }
      }
      this.timeSchedule = [[], [], [], [], [], [], []]
      this.isSelectSchedule = false
    },
    changePropsValue () {
      const str = this.num.join('')
      const data = str.replace(RegExp(',', 'g'), '')
      this.$emit('input', data)
    },
    showSchedule () {
      this.isSelectSchedule = false
      for (let x = 0; x < this.num.length; x++) {
        // 存放每星期的时间段
        let weekScheduleList = []
        // 记录上一个合并时间段数组所在位置
        let index = 0
        // 记录当前显示的时间段
        let curTime = '' 

        for (let y = 0; y < 48; y++) {
          let startTime = ''
          let endTime = ''
          let selected = this.num[x][y]
          if (selected) {
            this.isSelectSchedule = true
            if (y % 2 === 0) {
              startTime = this.timeTitle[y] + ':00'
              endTime = this.timeTitle[y] + ':30'
              curTime = startTime + '~' + endTime
            } else {
              startTime = this.timeTitle[y] + ':30'
              endTime = this.timeTitle[y + 1] + ':00'
              curTime = startTime + '~' + endTime
            }
            if (this.num[x][y - 1] === 1) {
              // 获取上一次合并时间段的数据，把时间拆成两段，取前一段时间
              let preTime = weekScheduleList[index - 1].split('~')[0]
              weekScheduleList.splice(index - 1, 1)
              curTime = preTime + '~' + endTime
              index = index - 1            
            } 
            weekScheduleList.push(curTime)
            index = index + 1
          }
        }
        this.timeSchedule[x] = weekScheduleList.length > 0 ? weekScheduleList : []
      }
    }
  },
  filters: {
  formatItem: function (value) {
    if (!value) return ''
    value = value.join('、')
    return value
  }
}
}
</script>

<style scoped>
  .byted-weektime .calendar {
    -webkit-user-select: none;
    position: relative;
    display: inline-block;
  }

  .byted-weektime .calendar .calendar-table {
    border-collapse: collapse;
    border-radius: 4px;
  }

  .byted-weektime .calendar .calendar-table tr .calendar-atom-time:hover {
    background: #ccc;
  }

  .byted-weektime .calendar .calendar-table tr .ui-selected {
    background: #2f88ff;
  }

  .byted-weektime .calendar .calendar-table tr .ui-selected:hover {
    background: #2f88ff;
  }

  .byted-weektime .calendar .calendar-table tr,
  .byted-weektime .calendar .calendar-table td,
  .byted-weektime .calendar .calendar-table th {
    border: 1px solid #e4e9ed;
    font-size: 12px;
    text-align: center;
    min-width: 11px;
    line-height: 1em;
    -webkit-transition: background 200ms ease;
    -moz-transition: background 200ms ease;
    -ms-transition: background 200ms ease;
    transition: background 200ms ease;
  }

  .byted-weektime .calendar .calendar-table tbody tr {
    height: 30px;
  }

  .byted-weektime .calendar .calendar-table tbody tr td:first-child {
    background: #f8f9fa;
  }

  .byted-weektime .calendar .calendar-table thead th,
  .byted-weektime .calendar .calendar-table thead td {
    background: #f8f9fa;
  }

  .byted-weektime .calendar .calendar-table .td-table-tip {
    line-height: 2.4em;
    padding: 0 12px 0 19px;
    background: #fff !important;
  }

  .byted-weektime .calendar .calendar-table .td-table-tip .clearfix {
    height: 46px;
    line-height: 46px;
  }

  .byted-weektime .calendar .calendar-table .td-table-tip .pull-left {
    font-size: 14px;
    color: #333333;
  }

  .byted-weektime .week-td {
    width: 75px;
    padding: 20px 0;
  }

  .byted-weektime a {
    cursor: pointer;
    color: #2f88ff;
    font-size: 14px;
  }

  #kuang {
    position: absolute;
    background-color: blue;
    opacity: 0.3;
  }

  .bg-purple {
    background: #d3dce6;
  }
</style>
