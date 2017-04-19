<template>
	<div class="pull-con" @touchstart='touchStart' @touchmove='touchMove' @touchend='touchEnd' :style='{"transform": "translate(0," + translate + "px )"}'>
		<div class="pull" v-show="state == 'pullTop'">
      {{loadState ? '松开刷新' : '继续下拉刷新'}}
    </div>
		<slot name='topLoad'>
			<div class="loading" v-show="loadingShow && catchState == 'pullTop'">刷新中...</div>
    </slot>
		<slot name='list'></slot>
		<div class="pull" v-show="state == 'pullDown'">{{loadState ? '松开刷新' : '继续下拉刷新'}}</div>
		<div v-show="loadingShow && catchState == 'pullDown'" ref='pullDown'>
			<slot name='bottomLoad'>
				<div class="loading" style='height:30px'>刷新中...</div>
	    </slot>
		</div>
	</div>
</template>

<script>
export default {
  props: {
    type: {
      type: String,
      default: 'all'
    },
    loadingShow: {
      type: Boolean,
      default: false
    },
    loadHeight: {
      type: Number,
      default: 40
    },
    moveTime: {
      type: Number,
      default: 300
    }
  },

  watch: {
    loadingShow(v) {
      if (!v) {
        this.catchState = '';
        this.bottomHeight = 0;
      }
    }
  },

  data() {
    return {
      scrollTag: null,
      maxScroll: 0,
      touch: null,
      state: '',
      catchState: '',
      loadState: false,
      translate: 0,
      bottomHeight: 0,
      startTime: 0
    };
  },

  methods: {
    touchStart(e) {
      if (!this.catchState && e.touches.length === 1) {
        this.touch = e.touches[0];
        this.startTime = Date.now();
      };
    },
    touchMove(e) {
      if (this.state) {
        e.preventDefault();
        var height = e.touches[0].clientY - this.touch.clientY;
        var coeff = Math.abs(height) > 5 ? 2 / Math.log(Math.abs(height)) : 1;
        this.translate = height * coeff;
        if (Math.abs(this.translate) > this.loadHeight) {
          this.loadState = true;
        }
      } else if (this.touch && ((this.scrollTag.scrollTop <= 0 && this.type !== 'bottom') ||
        (this.type !== 'top' && this.scrollTag.scrollTop >= this.maxScroll))) {
        var moveX = e.touches[0].clientX - this.touch.clientX;
        var moveY = e.touches[0].clientY - this.touch.clientY;
        if (Math.abs(moveX) < Math.abs(moveY / 2)) {
          if (moveY > 0 && this.scrollTag.scrollTop <= 0) {
            this.state = 'pullTop';
          } else if (moveY < 0 && this.scrollTag.scrollTop >= this.maxScroll) {
            this.state = 'pullDown';
          }
        }
      };

    },
    touchEnd(e) {
      // console.log(3);
      if (this.loadState && Date.now() - this.startTime >= this.moveTime) {
        // emit event
        console.log(2);
        this.catchState = this.state;
        this.$emit('loading', this.state);
        if (this.state === 'pullDown') {
          var that = this;
          setTimeout(function() {
            that.bottomHeight = that.$refs.pullDown.offsetHeight;
            that.scrollTag.scrollTop += that.bottomHeight;
          }, 0);
        }
      };
      if (this.state) {
        this.touch = null;
        this.state = '';
        this.loadState = false;
        this.translate = 0;
      };

    }
  },

  mounted() {
    var target = this.$el;
    var overFlow = '';
    while (!(overFlow === 'auto' || overFlow === 'scroll' || target.nodeName === 'BODY')) {
      if (overFlow) {
        target = target.parentNode;
      };
      overFlow = window.getComputedStyle(target).overflow;
    }
    this.maxScroll = target.scrollHeight - target.offsetHeight;
    this.scrollTag = target;
  }
};
</script>
 <style scoped>

 </style>
