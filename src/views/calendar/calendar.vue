<template>
    <div class="calendar-cont">
      <div class="calendar" id="calendar"></div>
    </div>
</template>

<script>
    export default {
      data () {
          return {}
      },
      mounted (){
        this.renderHtml()
        this.showCalendarData()
        this.bindEvent()
      },
      methods: {
        renderHtml () {
          let calendar = document.getElementById("calendar");
          let titleBox = document.createElement("div");  // 标题盒子 设置上一月 下一月 标题
          let bodyBox = document.createElement("div");  // 表格区 显示数据

          // 设置标题盒子中的html
          titleBox.className = 'calendar-title-box';
          titleBox.innerHTML = "<span class='prev-month' id='prevMonth'></span>" +
            "<span class='calendar-title' id='calendarTitle'></span>" +
            "<span id='nextMonth' class='next-month'></span>";
          calendar.appendChild(titleBox);    // 添加到calendar div中

          // 设置表格区的html结构
          bodyBox.className = 'calendar-body-box';
          let _headHtml = "<tr>" +
            "<th>日</th>" +
            "<th>一</th>" +
            "<th>二</th>" +
            "<th>三</th>" +
            "<th>四</th>" +
            "<th>五</th>" +
            "<th>六</th>" +
            "</tr>";
          let _bodyHtml = "";

          // 一个月最多31天，所以一个月最多占6行表格
          for(let i = 0; i < 6; i++) {
            _bodyHtml += "<tr>" +
              "<td></td>" +
              "<td></td>" +
              "<td></td>" +
              "<td></td>" +
              "<td></td>" +
              "<td></td>" +
              "<td></td>" +
              "</tr>";
          }
          bodyBox.innerHTML = "<table id='calendarTable' class='calendar-table'>" +
            _headHtml + _bodyHtml +
            "</table>";
          // 添加到calendar div中
          calendar.appendChild(bodyBox);
        },
        showCalendarData () {
          let _year = this.dateObj.getDate().getFullYear();
          let _month = this.dateObj.getDate().getMonth() + 1;
          let _dateStr = this.getDateStr(this.dateObj.getDate());

          // 设置顶部标题栏中的 年、月信息
          let calendarTitle = document.getElementById("calendarTitle");
          let titleStr = _dateStr.substr(0, 4) + "年" + _dateStr.substr(4,2) + "月";
          calendarTitle.innerText = titleStr;

          // 设置表格中的日期数据
          let _table = document.getElementById("calendarTable");
          let _tds = _table.getElementsByTagName("td");
          let _firstDay = new Date(_year, _month - 1, 1);  // 当前月第一天
          for(let i = 0; i < _tds.length; i++) {
            let _thisDay = new Date(_year, _month - 1, i + 1 - _firstDay.getDay());
            let _thisDayStr = this.getDateStr(_thisDay);
            _tds[i].innerText = _thisDay.getDate();
            //_tds[i].data = _thisDayStr;
            _tds[i].setAttribute('data', _thisDayStr);
            if(_thisDayStr == this.getDateStr(new Date())) {    // 当前天
              _tds[i].className = 'currentDay';
            }else if(_thisDayStr.substr(0, 6) == this.getDateStr(_firstDay).substr(0, 6)) {
              _tds[i].className = 'currentMonth';  // 当前月
            }else {    // 其他月
              _tds[i].className = 'otherMonth';
            }
          }
        },
        bindEvent () {
          let prevMonth = document.getElementById("prevMonth");
          let nextMonth = document.getElementById("nextMonth");
          this.addEvent(prevMonth, 'click', this.toPrevMonth);
          this.addEvent(nextMonth, 'click', this.toNextMonth);
          let table = document.getElementById("calendarTable");
          let tds = table.getElementsByTagName('td');
          for(let i = 0; i < tds.length; i++) {
            this.addEvent(tds[i], 'click', function(e){
              console.log(e.target.getAttribute('data'));
            });
          }
        },
        addEvent (dom, eType, func) {
          if(dom.addEventListener) {  // DOM 2.0
            dom.addEventListener(eType, function(e){
              func(e);
            });
          } else if(dom.attachEvent){  // IE5+
            dom.attachEvent('on' + eType, function(e){
              func(e);
            });
          } else {  // DOM 0
            dom['on' + eType] = function(e) {
              func(e);
            }
          }
        },
        toPrevMonth () {
          let date = this.dateObj.getDate();
          this.dateObj.setDate(new Date(date.getFullYear(), date.getMonth() - 1, 1));
          this.showCalendarData();
        },
        toNextMonth () {
          let date = this.dateObj.getDate();
          this.dateObj.setDate(new Date(date.getFullYear(), date.getMonth() + 1, 1));
          this.showCalendarData();
        },
        getDateStr (date) {
          let _year = date.getFullYear();
          let _month = date.getMonth() + 1;    // 月从0开始计数
          let _d = date.getDate();
          _month = (_month > 9) ? ("" + _month) : ("0" + _month);
          _d = (_d > 9) ? ("" + _d) : ("0" + _d);
          return _year + _month + _d;
        }
      },
      computed: {
        dateObj: function () {
          let _date = new Date();    // 默认为当前系统时间
          return {
            getDate : function(){
              return _date;
            },
            setDate : function(date) {
              _date = date;
            }
          }
        }
      }
    }
</script>

<style scoped lang="less" rel="stylesheet/less">
  .calendar-cont{
    /**
   * 设置日历的大小
   */
    .calendar{
      width: 220px;
      height: auto;
      display: block;
    }
  }
</style>
