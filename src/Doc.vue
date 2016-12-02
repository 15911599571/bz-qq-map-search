<template>
  <div class="ui container segment">
    <h1>{{name}}</h1>
    <p>{{desc}}</p>
    <table class="ui celled table">
      <thead>
        <tr><th>参数</th><th>说明</th></tr>
      </thead>
      <tbody>
        <tr v-show="parm_desc">
          <td colspan="2" v-html="parm_desc"></td>
        </tr>
        <tr v-for="parm in parms"> <td class="single line"> {{parm.parm}} </td> <td> {{parm.desc}} </td></tr>
        <tr v-show="parms.length===0">
          <td colspan="2"></td>
        </tr>
      </tbody>
    </table>
    <code v-text="code"></code>
    <div class="ui divider"></div>
    <map-search :map="map" ></map-search>
    <q-map style="height: 500px;" v-model="map" class="map"></q-map>
  </div>
</template>

<script>
  import 'weui/dist/style/weui.css'

  import MapSearch from './Bz.vue'
  import QMap from 'bz-qq-map'
  export default {
    components: {
      QMap,
      MapSearch
    },
    data: function () {
      return {
        name: 'bz-oauth2-button',
        desc: '生成oauth2 button',
        parm_desc: ` 依赖 semantic css
        <code>
        import 'semantic-ui/dist/components/button.css'
        import 'semantic-ui/dist/components/icon.css'</code>
        `,
        oauths: [
          'vk',
          'douban',
          'youtube',
          'facebook',
          'google plus',
          'twitter',
          'github',
          'instagram'
        ],
        parms: [
          {parm: 'oauths', desc: '一个list, 指定要显示的oauth方式:' + `['vk', 'twitter']`}
        ],
        code: '<oauth2 :oauths="oauths"></oauth2>',
        map: {}
      }
    },
    methods: {
    }
  }
</script>
