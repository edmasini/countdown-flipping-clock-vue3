<template>
  <div class="flip-container" :style="cssStyles">
    <div class="flip-card">
      <div class="flip-card-inner" :class="rotateClass">
        <div class="flip-card-front">
          <div class="text">
            <svg viewBox="0 0 15 15">
              <text x="50%" y="60%">{{ time }}</text>
            </svg>
          </div>
        </div>
        <div class="flip-card-back">
          <div class="text">
            <svg viewBox="0 0 15 15">
              <text x="50%" y="60%">{{ next }}</text>
            </svg>
          </div>
        </div>
      </div>
    </div>
    <div class="flip-card"></div>
    <div class="flip-card bottom">
      <div class="flip-card-inner upper" style="">
        <div class="flip-card-front">
          <div class="text">
            <svg viewBox="0 0 15 15">
              <text x="50%" y="60%">{{ next }}</text>
            </svg>
          </div>
        </div>
      </div>
    </div>
    <div class="flip-card bottom">
      <div class="flip-card-inner down" :class="rotateClass">
        <div class="flip-card-back">
          <div class="text">
            <svg viewBox="0 0 15 15">
              <text x="50%" y="60%">{{ time }}</text>
            </svg>
          </div>
        </div>
      </div>
    </div>
    <div class="label">{{ label }}</div>
  </div>
</template>

<style lang="scss" scoped>
$--bg-color: var(--bg-color);
$--gap: calc(var(--font-size) / 100);
$--font-size: var(--font-size);

.flip-container {
  position: relative;
  width: $--font-size; //140px;
  min-width: 100px;
  z-index: 0;
}

.flip-card {
  padding: 0 2%;
  margin: auto 0;
  // background-color: transparent;
  width: 100%;
  aspect-ratio: var(--aspect-ratio) / 1;
  -webkit-perspective: 50em;
  -moz-perspective: 50em;
  perspective: 50em;
  z-index: 2;
  &:not(.bottom) {
    -webkit-transform: translateZ(10px);
    transform: translateZ(10px);
  }
}
.bottom {
  position: absolute;
  top: 0;
  z-index: -1;
}

.flip-card-inner {
  position: relative;
  width: 100%;
  height: calc(100% + 2px);
  text-align: center;
  transition: transform 0.8s;
  -webkit-transform-style: preserve-3d;
  transform-style: preserve-3d;
  -webkit-transform-origin: bottom center;
  transform-origin: bottom center;
  transition-duration: 0.8s;
  z-index: 1;
  border-bottom: 1px solid transparent;
  @media (min-width: 599.99px) {
    border-bottom: $--gap solid transparent;
  }
  @media (min-width: 1439.99px) {
    border-bottom: 2px solid transparent;
  }
  &.down {
    -webkit-transform: rotateX(-180deg);
    transform: rotateX(-180deg);
    & > .flip-card-back::after {
      position: absolute;
      top: -5px;
      left: 0;
      width: 100%;
      height: 5px;
      content: " ";
      background-color: $--bg-color;
      opacity: var(--shadow-opacity);
      filter: brightness(10%) blur(1px);
      z-index: 999;
    }
  }
}

.flip-card-front,
.flip-card-back {
  position: absolute;
  width: 100%;
  height: 101%;
  -webkit-backface-visibility: hidden; /* Safari */
  -moz-backface-visibility: hidden;
  backface-visibility: hidden;
  overflow: hidden;
  background-color: $--bg-color;
}

.flip-card-front {
  z-index: 2;
  border-radius: 5% 5% 0 0 / 17.5% 17.5% 0 0;
  filter: brightness(95%);
}

.flip-card-back {
  z-index: 1;
  border-radius: 0 0 5% 5% / 0 0 17.5% 17.5%;
  -webkit-transform: rotateX(180deg);
  transform: rotateX(180deg);
  & > .text {
    -webkit-transform: translatey(-100%);
    transform: translatey(-100%);
  }
}

.text {
  position: relative;
  width: 100%;
  height: 100%;
}

.rotate {
  -webkit-transform: rotateX(-180deg); //translateY(-$--gap);
  -moz-transform: rotateX(-180deg); //translateY(-$--gap);
  transform: rotateX(-180deg); //translateY(-$--gap);
  & > .flip-card-back::after {
    transform: scaleY(20);
    transition: transform 0.8s;
  }
}

.rotate-back {
  -webkit-transform: rotateX(0deg);
  transform: rotateX(0deg);
  transition-property: none;
}

svg {
  width: 100%;
  height: 200%;
  -webkit-transform-origin: center;
  transform-origin: center;
  -webkit-transform: scale(0.9);
  transform: scale(0.9);
  text {
    text-anchor: middle;
    dominant-baseline: middle;
    fill: var(--font-color);
  }
}

.label {
  padding-top: 0.5rem;
  font-size: calc(var(--font-size) / 12);
  font-weight: 500;
  text-align: center;
}
</style>

<script>
export default {
  name: "Flipper",
  props: {
    value: Number,
    digits: {
      type: Number,
      default: 1,
    },
    bgColor: {
      type: String,
      default: "black",
    },
    fontColor: {
      type: String,
      default: "#e0e0e0",
    },
    shadowOpacity: {
      type: Number,
      default: 0.2,
    },
    fontSize: {
      type: String,
      default: "140px",
    },
    aspectRatio: {
      type: Number,
      default: 4,
    },
    label: String,
  },
  data() {
    return {
      time: [0].toLocaleString("en-US", {
        minimumIntegerDigits: this.digits,
        useGrouping: false,
      }),
      next: null,
      style: "",
      rotateClass: null,
    };
  },
  mounted() {},
  methods: {
    change(e) {
      if (e < 0) return;
      this.next = e.toLocaleString("en-US", {
        minimumIntegerDigits: this.digits,
        useGrouping: false,
      });
      this.rotateClass = "rotate";
      setTimeout(() => {
        this.time = this.next;
        this.rotateClass = "rotate-back";
      }, 800);
    },
  },
  watch: {
    value(val) {
      this.change(val);
    },
  },
  computed: {
    cssStyles() {
      return {
        "--bg-color": this.bgColor,
        "--font-color": this.fontColor,
        "--shadow-opacity": this.shadowOpacity,
        "--font-size": this.fontSize,
        "--aspect-ratio": this.aspectRatio,
      };
    },
  },
};
</script>
