<template>
  <transition name="fade">
    <div
      class="fixed right-0 bottom-0 m-12 w-12 h-12 cursor-pointer
      flex items-center justify-center rounded-full bg-mainColor text-white"
      v-show="visible"
      @click="backToTop">
      <svg viewBox="0 0 24 24" width="24" height="24"
        stroke="currentColor" stroke-width="2" fill="none"
        stroke-linecap="round" stroke-linejoin="round">
        <line x1="12" y1="19" x2="12" y2="5"></line>
        <polyline points="5 12 12 5 19 12"></polyline>
      </svg>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'BackToTop',
  data() {
    return {
      visible: false,
      visibleoffset: 500,
      visibleoffsetbottom: 0,
      scrollFn: () => [],
    };
  },
  mounted() {
    window.smoothscroll = () => {
      const currentScroll = document.documentElement.scrollTop || document.body.scrollTop;
      if (currentScroll > 0) {
        window.requestAnimationFrame(window.smoothscroll);
        window.scrollTo(0, Math.floor(currentScroll - (currentScroll / 5)));
      }
    };
    window.addEventListener('scroll', this.catchScroll);
  },
  destroyed() {
    window.removeEventListener('scroll', this.catchScroll);
  },
  methods: {
    catchScroll() {
      const visibleBottom = parseInt(this.visibleoffsetbottom, 10);
      const offHeight = document.body.offsetHeight;
      const pastTopOffset = window.pageYOffset > parseInt(this.visibleoffset, 10);
      const pastBottomOffset = window.innerHeight + window.pageYOffset >= offHeight - visibleBottom;
      this.visible = visibleBottom > 0 ? pastTopOffset && !pastBottomOffset : pastTopOffset;
      this.scrollFn(this);
    },
    backToTop() {
      window.smoothscroll();
      this.$emit('scrolled');
    },
  },
};
</script>
