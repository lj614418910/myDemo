<template>
  <div id="app">
    <ul class="wrapper"  ref="wrapper">
      <li class="list-wrapper" v-for = "(list, i) in lists">
        <v-List :list="list"></v-List>
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
export default {
  components: {
    'v-List' : List,
    'loading-icon' : loadingIcon
  },
  data(){
    return {
      lists: [
        {
          "id" : 0,
          "name" : "张一",
          "sex" : "男",
          "age" : 21,
          "class" : "三年级2班"
        },
        {
          "id" : 1,
          "name" : "李思",
          "sex" : "女",
          "age" : 20,
          "class" : "一年级3班"
        },
        {
          "id" : 2,
          "name" : "刘五一",
          "sex" : "男",
          "age" : 29,
          "class" : "二年级4班"
        },
        {
          "id" : 3,
          "name" : "安静",
          "sex" : "女",
          "age" : 11,
          "class" : "一年级4班"
        },
        {
          "id" : 4,
          "name" : "张二",
          "sex" : "男",
          "age" : 21,
          "class" : "五年级2班"
        },
        {
          "id" : 5,
          "name" : "吴丽",
          "sex" : "女",
          "age" : 22,
          "class" : "六年级2班"
        },
        {
          "id" : 6,
          "name" : "拉克",
          "sex" : "男",
          "age" : 19,
          "class" : "二年级2班"
        },
        {
          "id" : 7,
          "name" : "婉容",
          "sex" : "女",
          "age" : 21,
          "class" : "一年级1班"
        },
        {
          "id" : 8,
          "name" : "李丽",
          "sex" : "女",
          "age" : 22,
          "class" : "五年级2班"
        },
        {
          "id" : 9,
          "name" : "奥斯丁",
          "sex" : "男",
          "age" : 18,
          "class" : "四年级2班"
        },
        {
          "id" : 10,
          "name" : "吕一",
          "sex" : "男",
          "age" : 22,
          "class" : "四年级4班"
        }
      ]
    }
  },
  methods: {
    load(){
      this.$http.get('/api/list').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          response.data.forEach((item) =>{
            if(item != null){
              this.lists.push(item);
            }
          });
        };
      });
    },
    updata(){
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
      let _updata = this.updata;
      let _load = this.load;
      let scroll = new BScroll(this.$el, {
        probeType: 3
      });
      scroll.on('touchend', function (pos) {
        if (pos.y > 50) {
          setTimeout(function () {
            _updata();
            console.log(1);
            scroll.refresh()
          }, 500)
        };
        if (scroll.maxScrollY - pos.y > 60){
          setTimeout(function () {
            _load();
             console.log(2);
            scroll.refresh()
          }, 1000)
        }
      })
    }
  },
  created(){
    this.$nextTick(() => {
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
  }
}
</style>
