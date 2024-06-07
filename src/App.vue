<template>
  <section class="section-slider gsap_slider">
    <div class="content">
      <div class="section__slider gsap_h">
        <!-- gsap__bl -->
        <div class="gsap__bl">
          <div class="gsap__inner">
            <div class="gsap__track">
              <div
                v-for="(item, index) in items"
                :key="index"
                class="gsap__item"
              >
                <figure>
                  <img :src="item.src" :alt="item.alt" />
                </figure>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Lenis from "lenis";
import gsap from "gsap";
import ScrollTrigger from "gsap/ScrollTrigger";
import _ from "lodash";

export default {
  data() {
    return {
      items: [
        { src: require("./assets/01.jpg"), alt: "1" },
        { src: require("./assets/02.jpg"), alt: "2" },
        { src: require("./assets/03.jpg"), alt: "3" },
        { src: require("./assets/04.jpg"), alt: "4" },
        { src: require("./assets/06.jpg"), alt: "6" },
        { src: require("./assets/08.jpg"), alt: "8" },
      ],
      lenis: null,
    };
  },
  mounted() {
    this.initSmoothScroll();
    this.initScrollTrigger();
    window.addEventListener("resize", this.debouncedResize);
  },
  beforeDestroy() {
    window.removeEventListener("resize", this.debouncedResize);
  },
  methods: {
    initSmoothScroll() {
      this.lenis = new Lenis({
        duration: 1.2,
      });

      this.lenis.on("scroll", (e) => {
        console.log(e);
      });

      const raf = (time) => {
        this.lenis.raf(time);
        requestAnimationFrame(raf);
      };
      requestAnimationFrame(raf);

      this.lenis.on("scroll", ScrollTrigger.update);

      gsap.ticker.add((time) => {
        this.lenis.raf(time * 1000);
      });
    },
    initScrollTrigger() {
      gsap.registerPlugin(ScrollTrigger);

      let gsapBl = this.$el.querySelector(".gsap__bl").offsetWidth;
      let gsapTrack = this.$el.querySelector(".gsap__track").offsetWidth;
      let scrollSliderTransform = gsapTrack - gsapBl;

      gsap.to(".gsap__track", {
        scrollTrigger: {
          trigger: ".gsap_slider",
          start: "center center",
          end: () => "+=" + gsapTrack,
          pin: true,
          scrub: true,
        },
        x: "-=" + scrollSliderTransform + "px",
      });
    },
    onWindowResize() {
      console.log("Window resized!");
      location.reload();
    },
  },
  computed: {
    debouncedResize() {
      return _.debounce(this.onWindowResize, 500);
    },
  },
};
</script>

<style>
@import url(./assets/styles.css);
</style>
