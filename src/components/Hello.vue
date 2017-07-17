<template>
  <div class="hello">
    <div class="alert alert-warning" role="alert" v-if="error!=''">{{event.error}}</div>
    <div v-for="n in events.result" class="event">
      <div class="event_info">
          <h3>ID:{{n._id}}</h3>
          <h4>事件：{{n.title}}</h4>
          <p>时间：{{n.year}}年{{n.month}}月{{n.day}}日</p>
          <p>详情：{{n.des}}</p>
          <p>农历：{{n.lunar}}</p>
      </div>
      <div class="event_img">
        <img :src="n.pic" />
      </div>
    </div>

  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue'
  import VueResource from 'vue-resource'
  Vue.use(VueResource)
  Vue.http.options.emulateJSON = true
  export default {
    name: 'hello',
    data: function () {
      return {
        error: '',
        events: {
          error_code: '',
          reason: '',
          result: ''
        },
        dataUrl: '../../static/data.json'
      }
    },
    mounted: function () {
      this.getData()
    },
    methods: {
      getData: function () {
        this.$http({
          url: this.dataUrl,
          method: 'GET',
          headers: {
            'Content-Type': 'x-www-from-urlencoded'
          }
        })
          .then((response) => {
            var res = response.data
            this.events.error_code = res.error_code
            this.events.reason = res.reason
            this.events.result = res.result.reverse()
            console.log(this.events.error_code)
            console.log(this.events.reason)
            console.log(this.events.result)
          },
          (response) => {
            this.error = response.error_code
            console.log('failed')
          })
          .catch(function (response) {
            console.log(response)
          })
      }
    }
  }
</script>

<style>
  .event{
    display: block;
    width: 1200px;
    height: 300px;
    overflow: hidden;
    margin: 10px;
    border: 1px solid #999999;
  }

  .event ul{
    list-style: none;
  }

  .event_info{
    background-color: #aaaaaa;
    width: 600px;
    height: 100%;
    display: block;
    float: left;
    text-align: left;
  }

  .event_img{
    background-color: #dddddd;
    display: block;
    overflow: hidden;
    height: 300px;
    margin: 0 auto;
    text-align: center;
  }

  .event_img img{
    height: 100%;
  }

</style>
