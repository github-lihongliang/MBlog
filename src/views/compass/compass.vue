<template>
    <div class="compass" id="compass">
      <div class="s"
           v-bind:class="{xx:60-v===s}"
           v-for="(v,k) in 60"
           v-bind:style="{transform:'translate('+xc(sr,v+s,60)+'px,'+yc(sr,v+s,60)+'px) rotate('+dg(v+s,60)+'deg)'}">{{zh(60-v,1)}}秒</div>

      <div class="i" v-for="(v,k) in 60"
           v-bind:class="{xx:60-v===i}"
           v-bind:style="{transform:'translate('+xc(ir,v+i,60)+'px,'+yc(ir,v+i,60)+'px) rotate('+dg(v+i,60)+'deg)'}">{{zh(60-v,1)}}分</div>

      <div class="h" v-for="(v,k) in 24"
           v-bind:class="{xx:24-v===h}"
           v-bind:style="{transform:'translate('+xc(hr,v+h,24)+'px,'+yc(hr,v+h,24)+'px) rotate('+dg(v+h,24)+'deg)'}">{{zh(24-v,1)}}时</div>

      <div class="w" v-for="(v,k) in 7"
           v-bind:class="{xx:7-v===w}"
           v-bind:style="{transform:'translate('+xc(wr,v+w,7)+'px,'+yc(wr,v+w,7)+'px) rotate('+dg(v+w,7)+'deg)'}">星期{{wk[7-v]}}</div>

      <div class="d" v-for="(v,k) in 31"
           v-bind:class="{xx:31-k===d}"
           v-bind:style="{transform:'translate('+xc(dr,k+d,31)+'px,'+yc(dr,k+d,31)+'px) rotate('+dg(k+d,31)+'deg)'}">{{zh(31-k,1)}}日</div>

      <div class="m" v-for="(v,k) in 12"
           v-bind:class="{xx:12-k===m+1}"
           v-bind:style="{transform:'translate('+xc(mr,k+m+1,12)+'px,'+yc(mr,k+m+1,12)+'px) rotate('+dg(k+m+1,12)+'deg)'}">{{zh(12-k,1)}}月</div>

      <div class="y">
        {{y}}年
      </div>
    </div>
</template>

<script>
  export default {
    data () {
      return {
        PI:Math.PI,
        s:0,
        i:0,
        h:0,
        d:0,
        m:0,
        y:0,
        w:0,
        sr:600,
        ir:500,
        hr:400,
        wr:300,
        dr:200,
        mr:100,
        wk:['日','一','二','三','四','五','六']
      }
    },
    mounted (){
      setInterval(()=>{
        let date=new Date()
        this.y=date.getFullYear()
        this.m=date.getMonth()
        this.d=date.getDate()
        this.w=date.getDay()
        this.h=date.getHours()
        this.i=date.getMinutes()
        this.s=date.getSeconds()
        Math.cos()
      },1000)
    },
    methods: {
      xc(r,k,l){
        return r*Math.cos(2*k*this.PI/l)
      },
      yc(r,k,l){
        return r*Math.sin(2*k*this.PI/l)
      },
      dg(k,l){
        return k*360/l
      },
      zh(num,type){
        switch(type){
          case 1:
            let t1=['零','一','二','三','四','五','六','七','八','九','十']
            let sw=parseInt(num/10)
            let gw=num%10
            return (sw!==0?t1[sw]+'十':'')+(gw!==0?t1[gw]:(sw===0?t1[0]:''))
            break
        }
      }
    }
  }
</script>

<style lang="less" rel="stylesheet/less">
  .compass{
    background-attachment: fixed;
    color: #F5F4F6;
    font-size: 12px;
    font-weight: bold;
    width: 700px;
    height: 600px;
    opacity: .3;
    position: absolute;
    top:126px;
    left: 94px;
    .s,.i,.h,.w,.d,.m,.y{
      position: absolute;
      top: 600px;
      left: 600px;
      transition:transform 1s;
      color: #999999;
    }
    .xx,.y{
      color: #FF0000;
      font-size: 14px;
      font-weight: bold;
    }
  }
</style>
