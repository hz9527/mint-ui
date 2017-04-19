<template>
	<div class='my-pull'>
		<mypull>
      <div class="list-con" slot='list'>
      	<div class="item" v-for='item in list'>{{item}}</div>
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
      list: []
    };
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
}
.item{
  height:40px;
  line-height: 40px;
  text-align: center;
  border-bottom: 1px solid #e55;
}
</style>
