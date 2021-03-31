<template>
  <div>
    <div
        class="pg"
        :style="{top: data.co.top + 'px', left: data.co.left + 'px', width: width + 'px', height: height + 'px', 'shape-outside': 'polygon(' + shape + ')', 'clip-path' : 'polygon(' + shape + ')'}"
    >
    </div>
    <div
        class="atooltip"
        :style="{top: data.co.top - 84 + 'px', left: data.co.left  + 'px'}"
    >
      <img :src="require(`@/assets/${data.img}`)" />
    </div>
  </div>
</template>
<script>
export default {
  props: {
    data: {
      type: Object
    }
  },
  computed: {
    width() {
      const polygon = this.data.polygon;
      let max = 0;
      polygon.forEach(item => {
        if(item[0] > max)
          max = item[0]
      })
      return max
    },
    height() {
      const polygon = this.data.polygon;
      let max = 0;
      polygon.forEach(item => {
        if(item[1] > max)
          max = item[1]
      })
      return max
    },
    polygon() {
      const areas = this.data.polygon;
      let res = [];
      for (const r in areas) {
        res.push(areas[r][0] + ',' + areas[r][1])
      }
      return res.join(' ');
    },
    shape() {
      const areas = this.data.polygon;
      let res = [];
      for (const r in areas) {
        res.push(areas[r][0] + 'px ' + areas[r][1] + 'px')
      }
      return res.join(', ');
    },
  }
}
</script>
<style lang="scss">
svg {
  position: absolute;
}
.pg {
  position: absolute;
  z-index: 999;
  &:hover + .atooltip {
    display: block;
  }
}
.atooltip {
  position: absolute;
  display: none;
  z-index: 10;
  border: 1px solid #fff;
  border-radius: 12px;
  overflow: hidden;
  img {
    max-width: 125px;
    display: block;
  }
}
</style>
