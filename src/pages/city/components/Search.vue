<template>
<div>
    <div class="search">
        <input type="text" class="search-input" placeholder="输入城市名/拼音"
        v-model="keyword"
        >
    </div>
    <div class="content" 
    ref="search"
    v-show="keyword"
    >
    <ul >
        <li class="item" 
        border-bottom
        v-for="item of list " 
        :key="item.id"
        @click="handleCityClick(item.name)"
        >
            {{item.name}}
        </li>       
        <li 
        class="item" 
        border-bottom
        v-show="hasNoData"
        >没有找到匹配数据</li>
    </ul>
    </div>
    </div>
</template>
<script>
import Bscroll from 'better-scroll'
export default {
  name: "CitySearch",
  props: {
    cities: Object
  },
  data() {
    return {
      keyword: "",
      list: [],
      tiemr: null
    };
  },
  methods:{
     handleCityClick (city){
         this.$store.commit('changeCity',city)
         this.$router.push('/')
      }
  },
  computed: {
    hasNoData () {
      return !this.list.length
    }
  },
  watch: {
    keyword() {
      if (this.timer) {
        clearTimeout(this.tiemr);
      }
      if(!this.keyword) {
        this.list = []
        return
      }
      this.timer = setTimeout(() => {
        const result = [];
        for (let i in this.cities) {
          this.cities[i].forEach(value => {
            if (
              value.spell.indexOf(this.keyword) > -1 ||
              value.name.indexOf(this.keyword) > -1
            ) {
              result.push(value);
            }
          });
        }
        this.list = result;
      }, 100);
    }
  },
  
  mounted () {
    this.scroll = new Bscroll(this.$refs.search,{click:true})
  }
};
</script>
<style lang="stylus" scoped>
@import '~styles/varibles.styl'

.search
    height .72rem
    background-color $bgColor
    padding 0 0.1rem
    .search-input
        box-sizing border-box
        width 100%
        height 0.62rem
        padding 0 0.1rem
        text-align center
        border-radius .06rem
        color #666
.content
    z-index 1
    position absolute
    top 1.58rem
    left 0
    right 0
    bottom 0
    background #eee
    
    .item
      line-height .62rem
      padding-left .2rem
      background #ffffff
      color #666
</style>

