<template>
  <main class="container selector" role="main">
    <!-- filtering circle -->
    <div class="selector-filter">
      <circle-svg
        svgid="areas"
        :vbox="vbox"
        :items="areas"
        :main-cx="mainCx"
        :main-cy="mainCy"
        :main-r="mainR"
        :text="text"
        :active-text="true"
        spacing="right"
        :display="[1, 1, 0]"
        @itemClicked="onClickChild"
      ></circle-svg>
    </div>
    <!-- filtered items -->
    <div class="selector-items">
      <div class="intro">
        <h2>Explore our solutions</h2>
        <p>
          MouBE provides simple and secure solutions in many fields. Each area
          provides a great variety of solutions for any need.
        </p>
        <p>
          Use the <strong>filter</strong> to see solutions from specific areas,
          get more information by opening the <strong>solution</strong> and
          contacting its <strong>referent</strong>.
        </p>
        <p>
          Each solution has an <strong>unique referent</strong>, and each
          referent only works for one <strong>unique solution</strong> - to
          ensure maximum performance and reliability for each MouBE solution,
          none excluded.
        </p>
      </div>
      <card-grid
        :cards="filteredListArea"
        shape="rectangle"
        path="/solutions"
      ></card-grid>
    </div>
  </main>
</template>

<script>
import CircleSvg from '~/components/CircleSvg.vue'

export default {
  components: { CircleSvg },
  async asyncData({ $axios }) {
    // about all areas
    const areas = await $axios.$get(`${process.env.BASE_URL}/api/area`)
    areas.sort((a, b) => (a.id > b.id ? 1 : b.id > a.id ? -1 : 0))
    // about all products
    const cards = await $axios.$get(`${process.env.BASE_URL}/api/product`)
    cards.sort((a, b) => (a.name > b.name ? 1 : b.name > a.name ? -1 : 0))
    // rename 'introShort' to 'description' so it works in the card component
    cards.forEach((card) => {
      card.description = card.introShort
      delete card.introShort
    })
    return {
      areas,
      cards,
    }
  },
  data() {
    return {
      // areas: [
      //   {
      //     name: 'Cloud computing',
      //     path: '#',
      //     color: '#F44336',
      //     icon: require('~/assets/icons/area-cloud-computing.svg?raw'),
      //     active: false,
      //   },
      //   {
      //     name: 'Analytics',
      //     path: '#',
      //     color: '#4CAF50',
      //     icon: require('~/assets/icons/area-analytics-2.svg?raw'),
      //     active: false,
      //   },
      //   {
      //     name: 'Machine Learning',
      //     path: '#',
      //     color: '#FFC107',
      //     icon: require('~/assets/icons/area-machine-learning.svg?raw'),
      //     active: false,
      //   },
      //   {
      //     name: 'Blockchain',
      //     path: '#',
      //     color: '#00BCD4',
      //     icon: require('~/assets/icons/area-blockchain.svg?raw'),
      //     active: false,
      //   },
      // ],
      // cards: [
      //   {
      //     title: 'Product ML',
      //     summary: 'Some area description',
      //     area: 'Machine Learning',
      //     image:
      //       'https://images.unsplash.com/photo-1472214103451-9374bd1c798e?ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8cGxhY2V8ZW58MHx8MHx8&ixlib=rb-1.2.1&w=1000&q=80',
      //   },
      //   {
      //     title: 'Product CC',
      //     summary: 'Some area description',
      //     area: 'Cloud computing',
      //     image:
      //       'https://images.unsplash.com/photo-1472214103451-9374bd1c798e?ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8cGxhY2V8ZW58MHx8MHx8&ixlib=rb-1.2.1&w=1000&q=80',
      //   },
      //   {
      //     title: 'Product CC',
      //     summary: 'Some area description',
      //     area: 'Cloud computing',
      //     image:
      //       'https://images.unsplash.com/photo-1472214103451-9374bd1c798e?ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8cGxhY2V8ZW58MHx8MHx8&ixlib=rb-1.2.1&w=1000&q=80',
      //   },
      //   {
      //     title: 'Product A',
      //     summary: 'Some area description',
      //     area: 'Analytics',
      //     image:
      //       'https://images.unsplash.com/photo-1472214103451-9374bd1c798e?ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8cGxhY2V8ZW58MHx8MHx8&ixlib=rb-1.2.1&w=1000&q=80',
      //   },
      //   {
      //     title: 'Product A',
      //     summary: 'Some area description',
      //     area: 'Analytics',
      //     image:
      //       'https://images.unsplash.com/photo-1472214103451-9374bd1c798e?ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8cGxhY2V8ZW58MHx8MHx8&ixlib=rb-1.2.1&w=1000&q=80',
      //   },
      //   {
      //     title: 'Product A',
      //     summary: 'Some area description',
      //     area: 'Analytics',
      //     image:
      //       'https://images.unsplash.com/photo-1472214103451-9374bd1c798e?ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8cGxhY2V8ZW58MHx8MHx8&ixlib=rb-1.2.1&w=1000&q=80',
      //   },
      //   {
      //     title: 'Product B',
      //     summary: 'Some area description',
      //     area: 'Blockchain',
      //     image:
      //       'https://images.unsplash.com/photo-1472214103451-9374bd1c798e?ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8cGxhY2V8ZW58MHx8MHx8&ixlib=rb-1.2.1&w=1000&q=80',
      //   },
      //   {
      //     title: 'Product B',
      //     summary: 'Some area description',
      //     area: 'Blockchain',
      //     image:
      //       'https://images.unsplash.com/photo-1472214103451-9374bd1c798e?ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8cGxhY2V8ZW58MHx8MHx8&ixlib=rb-1.2.1&w=1000&q=80',
      //   },
      //   {
      //     title: 'Product B',
      //     summary: 'Some area description',
      //     area: 'Blockchain',
      //     image:
      //       'https://images.unsplash.com/photo-1472214103451-9374bd1c798e?ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8cGxhY2V8ZW58MHx8MHx8&ixlib=rb-1.2.1&w=1000&q=80',
      //   },
      //   {
      //     title: 'Product B',
      //     summary: 'Some area description',
      //     area: 'Blockchain',
      //     image:
      //       'https://images.unsplash.com/photo-1472214103451-9374bd1c798e?ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8cGxhY2V8ZW58MHx8MHx8&ixlib=rb-1.2.1&w=1000&q=80',
      //   },
      //   {
      //     title: 'Product B',
      //     summary: 'Some area description',
      //     area: 'Blockchain',
      //     image:
      //       'https://images.unsplash.com/photo-1472214103451-9374bd1c798e?ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8cGxhY2V8ZW58MHx8MHx8&ixlib=rb-1.2.1&w=1000&q=80',
      //   },
      //   {
      //     title: 'Product B',
      //     summary: 'Some area description',
      //     area: 'Blockchain',
      //     image:
      //       'https://images.unsplash.com/photo-1472214103451-9374bd1c798e?ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8cGxhY2V8ZW58MHx8MHx8&ixlib=rb-1.2.1&w=1000&q=80',
      //   },
      //   {
      //     title: 'Product B',
      //     summary: 'Some area description',
      //     area: 'Blockchain',
      //     image:
      //       'https://images.unsplash.com/photo-1472214103451-9374bd1c798e?ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8cGxhY2V8ZW58MHx8MHx8&ixlib=rb-1.2.1&w=1000&q=80',
      //   },
      // ],
      // circle svg params
      mainCx: 450,
      mainCy: 250,
      mainR: 115,
      vbox: '330 70 625 350',
      text: ['Select area to', 'filter its products'],
      // for filtering
      search: '',
      filterby: 0, // 0 = All
    }
  },
  head() {
    return {
      title: 'Solutions',
    }
  },
  computed: {
    filteredListSearch() {
      return this.cards.filter((card) => {
        return card.title.toLowerCase().includes(this.search.toLowerCase())
      })
    },
    filteredListArea() {
      if (this.filterby === 0) {
        return this.cards
      } else {
        return this.cards.filter((card) => {
          // return !card.area.indexOf(this.filterby)
          return card.areaId === this.filterby
        })
      }
    },
  },
  mounted() {
    this.$store.commit('setTheme', 'default')
    this.$store.commit('setTitle', '')
    this.$store.commit('updateRoute', {
      title: 'Solutions',
      url: window.location.href,
    })
  },
  methods: {
    onClickChild(item) {
      if (this.filterby !== item.id) {
        this.filterby = item.id
      } else {
        this.filterby = 0
      }
    },
  },
}
</script>
