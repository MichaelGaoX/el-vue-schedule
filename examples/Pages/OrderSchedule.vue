<script>
// import Schedule from "../../plugins/Schedule";

import moment from "moment";
import Schedule from '../../packages/index.js'
//import Schedule from '../../lib/main.common.js'

export default {
  name: "OrderSchedule",
  components: {
    [Schedule.name]: Schedule
  },
  data() {
    return {
      year: "",
      rows: [
        {
          key: '01',
          title: "1月"
        },
        {
          key: '02',
          title: "2月"
        },
        {
          key: '03',
          title: "3月"
        },
        {
          key: '04',
          title: "4月"
        },
        {
          key: '05',
          title: "5月"
        },
        {
          key: '06',
          title: "6月"
        },
        {
          key: '07',
          title: "7月"
        },
        {
          key: '08',
          title: "8月"
        },
        {
          key: '09',
          title: "9月"
        },
        {
          key: '10',
          title: "10月"
        },
        {
          key: '11',
          title: "11月"
        },
        {
          key: '12',
          title: "12月"
        },],
      selectId: undefined,
      startDate: '',
      endDate: ''
    };
  },
  mounted() {
    // 初始化月份
    this.year = moment().format("YYYY");
  },
  methods: {

    // 重写单元格内容,
    cellRender(date, row) {
      const _this = this;
      // 获取所有事件
      const { key, events } = row || {};

      // 获取当前单元格日期的事件
      const dKey = date.format("YYYY-MM-DD");

      const ev = events && dKey in events ? events[dKey] : undefined;
      const { type = "", sn } = ev || {};


      // 过期
      let disable = "";
      // if (moment().isAfter(date)) {
      //   disable = "disabled";
      // }

      // 选中判断
      let active = "";
      //console.log(_this.startDate,'s')
      if (!type) {
        
        active = date.isSame(_this.startDate, "day") || date.isSame(_this.endDate, "day") || date.isBetween(_this.startDate, _this.endDate, "day") ? "active" : "";
        //console.log('active',_this.startDate,date.isSame('', "day"))
      }

      const thisDate = date.format("YYYY-MM-DD");
      return (
        <div
          class={`tag ${type} ${active} ${disable}`}
          onClick={_ => {
            (!type) && _this.onSelect(thisDate, row);
          }}
        >
          {dKey}
        </div>
      );
    },
    onChange(date) {
      //console.log('选择了:',date);
      this.month = date;
      this.getData(this.month);
    },
    // 当选择
    onSelect(date, row) {
      console.log(date, 'onSelect')
      const { key } = row || {};
      // // 禁此选择
      // if (moment().isAfter(date)) {
      //   return;
      // }
      // first click
      if (!this.startDate) {
        this.startDate = date;
        this.endDate = date;
        this.selectId = key;
      } else {
        if (date > this.startDate) {
          this.endDate = date;
        } else {
          this.endDate = this.startDate;
          this.startDate = date;
        }
      }
      console.log(this.startDate, this.endDate);
    }
  }
};
</script>
<template>
  <div class="order-schedule">
    <div>
      <ac-schedule title="月/日" :year="year" :rows="rows" :cellRender="cellRender" @change="onChange" />
    </div>
  </div>
</template>
<style scoped>
.order-schedule {
  margin: 25px;
}

.sc-schedule .cell .tag {
  display: inline-block;
  height: 100%;
  width: 100%;
}

.sc-schedule .cell .tag,
.example .tag {
  background: #fff;
  font-size: 0;
}

.sc-schedule .cell .tag.active {
  background: rgb(3, 117, 247);
  line-height: 30px;
}

.sc-schedule .cell .tag.active::after {
  font-size: 14px;
  line-height: 30px;
  font-family: "element-icons" !important;
  speak: none;
  font-style: normal;
  font-weight: normal;
  font-variant: normal;
  text-transform: none;
  vertical-align: baseline;
  display: inline-block;
  -webkit-font-smoothing: antialiased;
  content: "\e797";
  color: #fff;
}

.sc-schedule .cell .tag.disabled {
  background: #ccc;
}

.sc-schedule .cell .tag.review {
  background-color: rgba(245, 108, 108, 0.4) !important;
  border-color: rgba(245, 108, 108, 0.2);
}
.sc-schedule .cell .tag.order,
.example .tag.order {
  background-color: rgba(103, 194, 58, 0.4) !important;
  border-color: rgba(103, 194, 58, 0.2);
}

.sc-schedule .cell .tag.ssp,
.example .tag.ssp {
  background-color: rgba(230, 162, 60, 0.4) !important;
  border-color: rgba(230, 162, 60, 0.2);
}
</style>



