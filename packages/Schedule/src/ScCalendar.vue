<script>
import moment from "moment";
import Watermark from "awesome-watermark/vue";
export default {
  name: "ScCalendar",
  props: {
    // 初始月份
    year: {
      type: String,
      required: false
    },
    rows: {
      type: Array,
      required: false,
      default: _ => []
    },
    cellRender: {
      type: Function,
      required: false
    }
  },
  data() {
    return {
      //monthStr: ""
    };
  },
  watch: {
    year(newV) {
      if (newV) {
        this.year = newV;
      } else {
        this.year = moment().format('YYYY');
      }
    },
    // date() {
    //   this.monthStr = this.date.format("YYYY-MM");
    // }
  },

  mounted() {
    //console.log(moment(this.year))

  },
  methods: {
    // setDate(date) {
    //   this.year = date;
    //   this.monthStr = this.date.format("YYYY-MM");
    //   this.$emit('change', this.monthStr)
    // },
    prevYear() {
      this.setDate(this.year.add(-1, "years"));
    },
    nextMonth() {
      this.setDate(this.year.add(1, "years"));
    }
  },
  render(h) {
    // 总天数
    const days = 31;
    // console.log(days,'days')
    const randerDays = () => {
      let tds = [];
      let day = 1;
      //const date = this.date.date(1).clone();
      for (day; day <= days; day++) {
        //const w = date.day();
        // const d = 1;
        // const weekEnd = w === 0 || w === 6 ? 'weekend' : '';
        // const today = moment().isSame(date, 'day') ? 'today' : ''
        // class={`${weekEnd} ${today}`}
        tds.push(
          <th>
            <div class="cell">{day}</div>
          </th>
        );
        //date.add(1, "d");
      }
      return tds;
    };
    // const randerWeeks = () => {
    //   let tds = [];
    //   let day = 1;
    //   const date = this.date.date(1).clone();
    //   for (day; day <= days; day++) {
    //     const w = date.day();
    //     const str = ["日", "一", "二", "三", "四", "五", "六"][w];
    //     const weekEnd = w === 0 || w === 6 ? 'weekend' : '';
    //     tds.push(
    //       <th class={`${weekEnd}`}>
    //         <div class="cell">{str}</div>
    //       </th>
    //     );
    //     date.add(1, "d");
    //   }
    //   return tds;
    // };

    // 渲染单元格
    const cellRender = (date, row) => {
      return this.cellRender(date, row)
      if (this.cellRender && typeof (this.cellRender) == 'function') {

      }
      return ''
    }

    // 渲染行
    const rowRender = (row) => {
      //console.log(row,'row')
      const cells = [];
      // console.log(`2020-${row.key}`)
      // console.log(moment(moment(`2020-${row.key}`).format("YYYY-MM")).daysInMonth(),'999') 
      let rowMonth = moment(`${this.year}-${row.key}`,"YYYY-MM");
      let date = rowMonth.date(1).clone();
      for (let day = 1; day <= rowMonth.daysInMonth(); day++) {
        cells.push(<td><div class="cell">{cellRender(date, row)}</div></td>)
        date.add(1, "d");
      }
      return <tr>{cells}</tr>;
    }
    // <div class="sc-calendar-header">
    //       <button onClick={this.prevMonth}>上个月</button>
    //       <span>{this.monthStr}</span>
    //       <button onClick={this.nextMonth}>下个月</button>
    //     </div>
    return (
      <div class="sc-calendar">

        <div class="sc-calendar-table">
          <table cellpadding="0" cellspacing="0">
            <thead>
              <tr>{randerDays()}</tr>
            </thead>
            <tbody>
              {this.rows && this.rows.map((row) => {
                return rowRender(row)
              })}

              <Watermark class="water-mask"
                text="2017" row={1} col={1}
                angle={0}
                textStyle={{ color: "rgba(98, 98, 98,0.09)", fontSize: '463px', fontWeight: 400 }}
              />

            </tbody>
          </table>
        </div>
      </div>
    );
  }
};
</script>
<style >
.sc-calendar {
  position: relative;
}
.water-mask {
  top: 0;
  z-index: -1;
}
.sc-calendar-header,
.sc-calendar-table {
  width: 100%;
}
.tbody {
  position: relative;
}
.sc-calendar-header {
  height: 30px;
  background: #f3f3f3;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.sc-calendar-header button {
  margin: 2px 5px;
  font-size: 12px;
  padding: 3px 10px;
}
.sc-calendar-table {
  min-height: 300px;
  overflow: auto;
  padding-bottom: 30px;
}

.sc-calendar-table table {
  table-layout: fixed;
  min-width: 840px;
  border-collapse: collapse;
}

.sc-calendar-table td,
.sc-calendar-table th {
  border: 1px solid rgba(0, 0, 0, 0.3);
  vertical-align: middle;
  transition: background-color 0.2s ease;
  overflow: hidden;
  max-height: 30px;
  max-width: 30px;
  font-size: 0;
  padding: 0;
  margin: 0;
}

.sc-calendar-table tr td:first-child,
.sc-calendar-table tr th:first-child {
  border-left: 0;
}
.sc-calendar-table tr td:last-child,
.sc-calendar-table tr th:last-child {
  /* border-right: 0; */
}

.sc-calendar-table table td > .cell,
.sc-calendar-table table th > .cell {
  display: block;
  width: 30px;
  height: 30px;
  text-align: center;
  overflow: hidden;
  text-align: center;
  word-break: keep-all;
  text-overflow: ellipsis;
}

.sc-calendar-table table thead th {
  color: #606266;
  background: #f3f3f3;
  font-size: 12px;
  line-height: 30px;
}

.sc-calendar-table table thead .weekend {
  color: #2196f3;
}

.sc-calendar-table table thead th.today {
  background: #ffeb3b;
}

.sc-calendar-table table tbody td,
.sc-calendar-table table tbody th {
  cursor: pointer;
  background: #fff;
}
</style>

