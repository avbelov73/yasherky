<template>
  <div>
  <svg
      :viewBox="'0 0 ' + width + ' ' + height"
      :width="width"
      :height="height"
      :style="{top: area.co.top + 'px', left: area.co.left + 'px'}"
  >
    <polygon
        :points="polygon"
        :class="areaClass"
    ></polygon>
    <text x="50%" y="50%" dominant-baseline="middle" text-anchor="middle">{{area.id}}</text>
  </svg>
    <div
        class="pg"
        :style="{top: area.co.top + 'px', left: area.co.left + 'px', width: width + 'px', height: height + 'px', 'shape-outside': 'polygon(' + shape + ')', 'clip-path' : 'polygon(' + shape + ')'}"
        @mouseover="setActiveArea(data.id)"
        @mouseout="setActiveArea(null)"
    >
    </div>
    <div
        class="tooltip"
        :class="areaClass"
        :style="{top: area.co.top + (height / 3) - 84 + 'px', left: area.co.left + (width / 4)  + 'px'}"
    >
      <div
          class="tooltip-wrapper"
          :id="'tooltip-' + data.id"
          :class="areaClass"
      >
        <h4>{{status}}</h4>
        <div>Участок № {{data.id}}</div>
        <div>Площадь участка: {{data.area}} кв.м.</div>
        <div>Цена
          <template v-if="data.price && data.sold !== true">{{data.price}}</template>
          <template v-else>-</template>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    area: {
      type: Object
    },
    areaClass: {
      type: String
    },
    data: {
      type: Object
    }
  },
  computed: {
    polygon() {
      const areas = this.area.polygon;
      let res = [];
      for (const r in areas) {
        res.push(areas[r][0] + ',' + areas[r][1])
      }
      return res.join(' ');
    },
    shape() {
      const areas = this.area.polygon;
      let res = [];
      for (const r in areas) {
        res.push(areas[r][0] + 'px ' + areas[r][1] + 'px')
      }
      return res.join(', ');
    },
    width() {
      const polygon = this.area.polygon;
      let max = 0;
      polygon.forEach(item => {
        if(item[0] > max)
          max = item[0]
      })
      return max
    },
    height() {
      const polygon = this.area.polygon;
      let max = 0;
      polygon.forEach(item => {
        if(item[1] > max)
          max = item[1]
      })
      return max
    },
    status() {
      if(this.data.sold === 'Забронирован')
        return this.data.sold;
      else
        return (this.data.sold) ? 'Продан' : 'Свободен'
    }
  },
  methods: {
    setActiveArea(id) {
      this.$emit('active', id)
    }
  }
}
</script>
<style lang="scss" scoped>
svg {
  position: absolute;
  text {
    fill: #000;
    font-size: 8px;
    font-weight: bold;
  }
}
.success {
  fill: #00d800;
  background: #00d800;
  &:after {
    border-top-color: #00d800 !important;
  }
}
.warning {
  fill: #ffc600;
  background: #ffc600;
  &:after {
    border-top-color: #ffc600 !important;
  }
}
.danger {
  fill: #ff5100;
  background: #ff5100;
  &:after {
    border-top-color: #ff5100 !important;
  }
}
.pg {
  position: absolute;
  z-index: 999;
  &:hover + .tooltip {
    display: block;
  }
}
.tooltip {
  position: absolute;
  display: none;
  z-index:10;
  min-width: 195px;
  &-wrapper {
    color: #fff;
    padding: 8px 12px;
    border: 2px solid #fff;
    font-size: 12px;
    position: relative;
    h4 {
      margin: 0 0 8px;
    }
    &:before {
      content: '';
      width: 0;
      height: 0;
      border-left: 10px solid transparent;
      border-right: 10px solid transparent;
      border-top: 10px solid #fff;
      position: absolute;
      bottom: -10px;
      left: 0;
    }
    &:after {
      content: '';
      width: 0;
      height: 0;
      border-left: 7px solid transparent;
      border-right: 7px solid transparent;
      border-top: 7px solid transparent;
      position: absolute;
      bottom: -7px;
      left: 3px;
    }
  }

}
</style>
