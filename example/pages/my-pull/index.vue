<template>
	<div class="pull-con" @touchstart='touchStart' @touchmove='touchMove' @touchend='touchEnd'>
		<slot name='list'></slot>
	</div>
</template>

<script>
export default {
  props: {
    type: {
      type: String,
      default: 'all'
    }
  },

  data() {
    return {
      scrollTag: null,
      touch: null,
      state: '',
      catchState: '',
      translate: 0
    };
  },

  methods: {
    touchStart(e) {
      console.log(1);
    },
    touchMove(e) {
      console.log(2);
    },
    touchEnd(e) {
      console.log(3);
    }
  },

  mounted() {
    var target = this.$el;
    console.log(target);
    var overFlow = '';
    while (overFlow !== 'auto' && overFlow !== 'scroll') {
      if (overFlow) {
        target = target.parentNode;
      };
      console.log(target);
      overFlow = window.getComputedStyle(target).overflow;
      console.log(overFlow === 'visible');
    }

  }
};
</script>
 <style scoped>

 </style>
