<template>
  <button type="button" class="btn" :class="type">
    <slot></slot>
  </button>
</template>

<script>
  export default {
    name: 'LeButton',
    props: {
      type: {
        type: String,
        default: 'btn-primary'
      },
      endText: {
        type: String,
        default: '已结束'
      },
      callback: {
        type: Function,
        default: null
      },
      showType: {
        type: String,
        default: ''
      }
    },
    mounted () {
      this.countdowm(this.endTime);
    },
    methods: {
      countdowm(timestamp) {
        let self = this;
        let timer = setInterval(function() {
          let nowTime = new Date();
          let endTime = new Date(timestamp * 1000);
          let t = endTime.getTime() - nowTime.getTime();
          if (t > 0) {
            let format = self.format(t);
            self.content = format;
          } else {
            clearInterval(timer);
            self.content = self.endText;
            self._callback();
          }
        }, 1000);
      },
      format(t) {
        let day = Math.floor(t / 86400000);
        let hour = Math.floor((t / 3600000) % 24);
        let min = Math.floor((t / 60000) % 60);
        let sec = Math.floor((t / 1000) % 60);
        hour = hour < 10 ? '0' + hour : hour;
        min = min < 10 ? '0' + min : min;
        sec = sec < 10 ? '0' + sec : sec;
        let format = '';
        if (this.showType === 'Cn') {
          if (day > 0) {
            format = `${day}天${hour}小时${min}分${sec}秒`;
          }
          if (day <= 0 && hour > 0) {
            format = `${hour}小时${min}分${sec}秒`;
          }
          if (day <= 0 && hour <= 0) {
            format = `${min}分${sec}秒`;
          }
        } else if (this.showType === 'Time') {
          if (day > 0) {
            format = `${day} ${hour}:${min}:${sec}`;
          }
          if (day <= 0 && hour > 0) {
            format = `${hour}:${min}:${sec}`;
          }
          if (day <= 0 && hour <= 0) {
            format = `${min}:${sec}`;
          }
        } else {
          format = parseInt(t / 1000);
        }
        return format;
      },
      _callback() {
        if (this.callback && this.callback instanceof Function) {
          this.callback(...this);
        }
      }
    }
  };
</script>

<style scoped>

</style>
