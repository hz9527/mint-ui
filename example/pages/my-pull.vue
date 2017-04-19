<template>
	<div class='my-pull'>
		<mypull :loading-show='loading' @loading='load'>
      <div class="list-con" slot='list'>
      	<div class="item" v-for='item in list'>{{item}}</div>
      </div>
			<div class="load" slot='bottomLoad' style='height:30px'>
				...
			</div>
    </mypull>
	</div>
</template>

<script>
import Mypull from './my-pull/index.vue';
export default {
  components: {
    Mypull
  },

  data() {
    return {
      list: [],
      loading: false
    };
  },

  methods: {
    load(v) {
      var that = this;
      this.loading = true;
      setTimeout(function() {
        var index;
        if (v === 'pullTop') {
          index = that.list[0];
          for (var i = 0; i < 10; i++) {
            that.list.unshift(index - i);
          }
        } else {
          index = that.list[that.list.length - 1];
          for (var j = 0; j < 10; j++) {
            that.list.push(index + j);
          }
        };
        that.loading = false;
      }, 2000);
    }
  },

  created() {
    for (var i = 0; i < 20; i++) {
      this.list.push(i);
    };
  }
};
</script>

<style scoped>
.my-pull{
  margin-top: 50px;
	border: 1px solid #f55;
	height: 400px;
  overflow: auto;
}
.item{
  height:40px;
  line-height: 40px;
  text-align: center;
  border-bottom: 1px solid #e55;
}
</style>
