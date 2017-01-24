<template lang="html">
  <div v-show="list" class="list border-1px">
    <div class="top">
      <div class="name" :class="{'girl':list.sex == '女'}">
        {{list.name}}
      </div>
      <div class="id">
        学号:{{list.id}}
      </div>
    </div>
    <div class="bottom">
      <div class="sex">
        性别:{{list.sex}}
      </div>
      <div class="class">
        班级:{{list.class}}
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    list: {
      type: Object
    }
  },
}
</script>
<style lang="scss">
  @import "../../common/scss/mixin.scss";
  .list{
    @include border-1px(rgba(7, 17, 27, 0.1));
    padding: 8px 0px;
    .top{
      font-size: 0px;
      .name,.id{
        display: inline-block;
        font-size: 15px;
      }
      .name{
        margin: 3px;
        padding: 1px 5px;
        vertical-align: middle;
        &{
          border-left: 3px solid #59f;
        }
        &.girl{
          border-left: 3px solid #f33;
        }
      }
      .id{
        margin-left: 20px;
        vertical-align: middle;
      }
    }
    .bottom{
      font-size: 0px;
      .sex, .class{
        display: inline-block;
        font-size: 13px;
      }
      .sex{
        margin: 10px 4px 6px;

      }
      .class{
        margin-left: 10px;
      }
    }
  }
</style>
