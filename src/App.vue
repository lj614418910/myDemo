<template>
  <div id="app">
    <ul class="wrapper"  ref="wrapper">
      <li class="list-wrapper" v-for = "(list, i) in lists">
        <v-List :list="list"></v-List>
      </li>
      <li>
        <div class="load">
          {{loading}}
        </div>
      </li>
    </ul>
    <loading-icon></loading-icon>
  </div>
</template>

<script>
import BScroll from 'better-scroll';
import List from 'components/list/List';
import loadingIcon from 'components/loadingIcon/loadingIcon';
const ERR_OK = 0;
let _index1 = 1;
let _index2 = 1;
export default {
  components: {
    'v-List' : List,
    'loading-icon' : loadingIcon
  },
  data(){
    return {
      lists: [],
      loading: "正在加载中..."
    }
  },
  methods: {
    _load(){
      this.$http.get('/api/list').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          response.data.forEach((item) =>{
            if(item != null){
              this.lists.push(item);
            }else{
              this.loading = "没有更多了"
            }
          });
        };
      });
    },
    _updata(){
      this.$http.get('/api/list').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          response.data.forEach((item) =>{
            if(item != null){
              this.lists.unshift(item);
            }
          });
        };
      });
    },
    _initScroll() {
      let _updata = this._updata;
      let _load = this._load;
      let scroll = new BScroll(this.$el, {
        probeType: 1,
        momentum: false
      });
      scroll.on('touchend', function (pos) {
        scroll.refresh()
        if (_index1 && pos.y > 50) {
          _index1 = 0;
          setTimeout(function () {
            _updata();
            _index1 = 1;
          }, 500)
        };
        console.log(scroll.wrapperHeight - scroll.y - scroll.scrollerHeight)
        if (_index2 && scroll.wrapperHeight - scroll.y - scroll.scrollerHeight == 0){
          _index2 = 0;
          setTimeout(function () {
            _load();
            _index2 = 1;
          }, 500)
        }
      })
    }
  },
  created(){
    this.$nextTick(() => {
      this._load();
      this._load();
      this._initScroll();
    });
  }
}
</script>

<style lang="scss">

#app{
  position: absolute;
  background: #f5f5f5;
  top:0px;
  bottom: 0px;
  width: 100%;
  overflow: hidden;
  .wrapper{
    position: absolute;
    width: 100%;
    background: #fff;
    z-index: 1;
    .list-wrapper{
      padding: 0px 10px;
    }
    .load{
      padding: 10px;
      text-align: center;
    }
  }
}
</style>
