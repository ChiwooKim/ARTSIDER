<template>
<div>
  <Loading v-if="loading" />
  <div v-else>
    <br>
    <h1 class="recommend-title ml-4 mb-4 d-flex align-center" v-if="genre != 'main'"><v-icon size="48px" color="red">mdi-fire</v-icon>{{ genre }} 인기 순위</h1> 
    <h1 class="recommend-title ml-4 mb-4 d-flex align-center" v-if="genre == 'main'"><v-icon size="48px" color="red">mdi-fire</v-icon>인기 순위</h1> 
    <swiper class="swiper" 
      :options="swiperOption">
      <swiper-slide
      v-for="popular in popularList" 
      :key="popular.id"
      > 
      <v-card elevation="12" style="width:100%;height:100%;">
        <v-img
              @click="moveShowDetail(popular.id)"
              :aspect-ratio="3/4"
              :src="popular.posterPath"
              style="width:100%;height:100%;"
              >
        </v-img>
      </v-card>
      </swiper-slide>
      <div class="swiper-button-prev" slot="button-prev"></div> 
      <div class="swiper-button-next" slot="button-next"></div>
    </swiper>
    <br>
  </div>
</div>
</template>

<script>
import { Swiper, SwiperSlide } from 'vue-awesome-swiper'
import Loading from '@/views/show/Loading'

export default {
  name: 'Popular',
  data(){
    return{
      swiper: null,
      popularList: [],
      loading : true
    }
  },

  props:{
    swiperOption: Object,
    num: Number,
    genre: String 
  },

  components:{
    Swiper,
    SwiperSlide,
    Loading
  },

  methods: {
    getCategoryPopularShow: function(num) {
      this.$store.dispatch('getCategoryPopularShow', num)
      .then(res => {
        this.popularList = res.data.items
        this.loading = false
      })
    },
    moveShowDetail: function (id) {
      this.$router.push({name: `ShowDetail`, params: { showId: id}})
    }
  },
  created: function() {
    // 새로 시작할 때 발생하는 이벤트
    // 페이지 처음 들어올때 수행하는 이벤트
    this.getCategoryPopularShow(this.num) 
  }
}
</script>

<style scoped>
.rank {
  color: #000;
  -webkit-text-stroke: 1px #fff;
  position: absolute;
  font-size: 4rem;
}
.swiper-button-prev {
  color: red;
}
.swiper-button-next {
  color: red;
}
</style>