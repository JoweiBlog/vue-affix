<template>
  <div>
    <div ref="point" :class="cls" :style="styles">
      <slot></slot>
    </div>
    <div v-show="block" :style="blockStyle"></div>
  </div>
</template>

<script>
// dom
const on = (function() {
  return function(ele, evt, handle) {
    ele.addEventListener(evt, handle, false);
  };
})();

const off = (function() {
  return function(ele, evt, handle) {
    ele.removeEventListener(evt, handle, false);
  };
})();

function getScroll(target) {
  const { pageXOffset, pageYOffset } = target;
  const { scrollTop, scrollLeft } = window.document.documentElement;
  const { scrollTop: top, scrollLeft: left } = window.document.body;
  return {
    top: pageYOffset || scrollTop || top || 0,
    left: pageXOffset || scrollLeft || left || 0
  };
}

function getOffset(ele) {
  const { left, top } = ele.getBoundingClientRect();
  const { left: scrollLeft, top: scrollTop } = getScroll(window);

  const $body = window.document.body;
  const { clientTop = 0, clientLeft = 0 } = $body;

  return {
    top: top + scrollTop - clientTop,
    left: left + scrollLeft - clientLeft
  };
}

export default {
  name: "VAffix",
  computed: {
    cls() {
      return [
        {
          "v-affix": this.affix
        }
      ];
    },
    type() {
      let t = "top";
      if (this.offsetBottom >= 0) {
        t = "bottom";
      }
      return t;
    }
  },
  data() {
    return {
      affix: false,
      styles: {},
      block: false,
      blockStyle: {}
    };
  },
  props: {
    offsetTop: {
      type: Number,
      default: 0
    },
    offsetBottom: {
      type: Number
    }
  },
  mounted() {
    on(window, "scroll", this.handleScroll);
    on(window, "resize", this.handleScroll);
    this.$nextTick(() => {
      this.handleScroll();
    });
  },
  beforeDestroy() {
    off(window, "scroll", this.handleScroll);
    off(window, "resize", this.handleScroll);
  },
  methods: {
    handleScroll() {
      const affix = this.affix;
      const type = this.type;
      const offset = this[
        `offset${type.replace(type[0], type[0].toUpperCase())}`
      ];
      const elf = getOffset(this.$el);
      const { top: scrollTop } = getScroll(window);
      const winHeight = window.innerHeight;
      const elHeight = this.$refs.point.offsetHeight;
      const totleHeight = winHeight + scrollTop;

      if (
        !affix &&
        ((type === "top" && elf.top - offset < scrollTop) ||
          (type === "bottom" && elf.top + offset + elHeight > totleHeight))
      ) {
        this.affix = true;
        this.block = true;
        this.blockStyle = {
          width: `${this.$refs.point.clientWidth}px`,
          height: `${this.$refs.point.clientHeight}px`
        };
        this.styles = {
          [type]: `${offset}px`,
          left: `${elf.left}px`,
          width: `${this.$el.offsetWidth}px`
        };
      } else if (
        affix &&
        ((type === "top" && elf.top - offset > scrollTop) ||
          (type === "bottom" && elf.top + offset + elHeight < totleHeight))
      ) {
        this.affix = false;
        this.block = false;
        this.styles = null;
        this.blockStyle = {};
      }
    }
  }
};
</script>

<style>
.v-affix {
  position: fixed;
  z-index: 10;
}
</style>
