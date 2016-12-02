<template>
  <div class="search">
    <we-search-bar :search="search" :focus="clearOverlays" cancel="clearOverlays"></we-search-bar>
    <div v-show="markers.length>1" style='width: 100%; height: 300px' id="infoDiv"></div>
  </div>
</template>

<script>
  import WeSearchBar from 'bz-weui-search-bar'
  export default {
    watch: {
      'map': 'initSearch'
    },
    props: {
      location: {
        type: String,
        default: '云南'
      },
      map: {
        required: true
      }
    },
    components: {
      WeSearchBar
    },
    data: function () {
      return {
        keyword: '',
        markers: []
      }
    },
    ready () {
      // this.initSearch()
    },
    computed: {
    },
    methods: {
      initSearch: function () {
        var v = this
        this.search_service = new window.qq.maps.SearchService(
          {
            map: v.map,
            // 设置搜索范围
            location: v.location,
            // 设置搜索页码为1
            pageIndex: 0,
            // 设置每页的结果数为5
            pageCapacity: 4,
            // 设置展现查询结构到infoDIV上
            panel: document.getElementById('infoDiv'),
            // 设置动扩大检索区域。默认值true，会自动检索指定城市以外区域。
            autoExtend: true,
            // 检索成功的回调函数
            complete: function (results) {
              // 设置回调函数参数
              // console.log(results.detail)
              var pois = results.detail.pois
              var latlngBounds = new window.qq.maps.LatLngBounds()
              for (var i in pois) {
                var poi = pois[i]
                // 扩展边界范围，用来包含搜索到的Poi点
                latlngBounds.extend(poi.latLng)
                var marker = new window.qq.maps.Marker(
                  {
                    map: v.map,
                    position: poi.latLng
                  }
                )
                marker.setTitle(i + 1)
                v.markers.push(marker)
              }
              if (pois) {
                v.map.fitBounds(latlngBounds)
              } else {
                window.alert('未找到' + v.keyword)
              }
            },
            // 若服务请求失败，则运行以下函数
            error: function (e) {
              console.log(e)
              window.alert('出错了。')
            }
          }
        )
      },
      search: function (keyword) {
        this.keyword = keyword
        this.clearOverlays()
        if (keyword === '') {
          return
        }
        this.search_service.search(keyword)
      },
      clearOverlays: function () {
        var overlays = this.markers
        var overlay = overlays.pop()
        while (overlay) {
          overlay.setMap(null)
          overlay = overlays.pop()
        }
      }
    }
  }
</script>
