<template>
  <div id="app" :style="{backgroundImage: 'url(' + bg + ')'}">
    <div class="container">
      <template
          v-for="section in geo"
      >
        <Area
            v-for="area in section.places"
            :key="area.id"
            :area="area"
            :area-class="areaClass(area.id)"
            :data="prices.find(item => item.id === area.id)"
            @active="setActiveArea"
        />
      </template>
      <div class="container_bg" :style="{backgroundImage: 'url(' + border + ')'}"></div>
    </div>
    <div class="section" v-if="activeSection">
      <h4>Сектор {{section.id}}</h4>
      <div>Стоимость сотки {{section.price}} рублей</div>
      <div>{{section.places}}</div>
    </div>
  </div>
</template>

<script>
import c from '@/assets/2.png';
import border from '@/assets/border.png';
// import bg from '@/assets/bg.jpg';
import bg from '@/assets/1.png';
import Area from '@/components/Area.vue';
import geo from '@/geo.json';
import prices from '@/prices.json';
export default {
  name: 'App',
  data() {
    return {
      bg: bg,
      geo: geo,
      c: c,
      border: border,
      prices,
      activeSection: null
    }
  },
  components: {
    Area
  },
  computed: {
    section() {
      const section = this.geo.find(i => i.id === this.activeSection);
      let res = {
        id: section.id,
        price: section.price,
        places: ''
      }
      let places = section.places.map(i => i.id);
      let start = places[0];
      let counter = start;
      places.forEach(i => {
        if(i - counter > 1) {
          if (res.places.length > 0) {
            res.places += ', ';
          }
          res.places += start + ' - ' + counter;
          start = counter;
        }
        counter = i;
      })
      if(res.places.length === 0)
        res.places = start + ' - ' + counter;
      return res;
    }
  },
  methods: {
    click(event) {
      let container = document.querySelector('.container');
      console.log(event.pageX - container.offsetLeft, event.pageY - container.offsetTop);
    },
    areaClass(id) {
      const status = this.prices.find(price => price.id === id).sold;
      if(status === "Забронирован")
        return 'warning'

      return (status) ? 'danger' : 'success';
    },
    setActiveArea(id) {
      if(id !== null) {
        this.activeSection = this.geo.find(section => {
          let t = section.places.find(s => s.id === id)
          return (t) ? section.id : null
        }).id;
      } else {
        this.activeSection = null
      }
    }
  },
}
</script>

<style lang="scss" scoped>
body {
  margin: 0 !important;

}
#app {
  width: 867px;
  height: 800px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  position: relative;
  background-size: contain;
}
.container {
  width: 498px;
  height: 633px;
  position: absolute;
  top: 90px;
  left: 184px;
  &_bg {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
  }
}
.section {
  position: absolute;
  bottom: 50px;
  left: 50px;
  background: #00d800;
  color: #fff;
  padding: 8px 12px;
  font-size: 12px;
  border: 1px solid #fff;
  h4 {
    margin: 0 0 8px;
  }
  div {
    padding: 8px;
    border-top: 1px solid #fff;
  }
}
</style>
