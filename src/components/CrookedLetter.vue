<template>
  <span :style="getStyle()" class="flicker uppercase">{{letter}}</span>
</template>

<script>
export default {
  name: 'Letter',
  data() {
    const sinRand = Math.sin(this.pos + this.letter.charCodeAt(0));
    return {
      sinRand,
      flicker: this.flickerProp || ((sinRand * 1000.0) % 1) >= 0.5,
    };
  },
  props: {
    letter: String,
    flickerProp: {
      type: Boolean,
      default: false,
    },
    pos: Number,
  },
  methods: {
    getStyle() {
      const roundRand = Math.floor(this.sinRand * 10000.0) / 200000.0;
      const rotate = `rotate(${roundRand}rad)`;
      const style = {
        display: 'inline-block',
        'margin-left': '0.2rem',
        transform: rotate,
      };
      if (this.flicker === true) {
        style.animation = 'flicker 5s linear infinite';
        style['animation-delay'] = `${(Math.random() * this.pos) % 2.0}s`;
      }
      return style;
    },
  },
};
</script>
