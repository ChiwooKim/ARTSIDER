<template>
<div v-if="isShow">
  <br>
  <h1 class="recommend-title ml-4 mb-4 d-flex align-center"><v-icon size="48px" color="green">mdi-account-group</v-icon>{{ userNickname }}님과 유사한 유저가 본 공연</h1> 
  <swiper class="swiper" 
    :options="swiperOption">
    <swiper-slide
    v-for="sim in SimilarityShowList" 
    :key="sim.id"
    > 
    <v-img
          @click="moveShowDetail(sim.id)"
          :aspect-ratio="3/4"
          :src="sim.posterPath"
          style="width:50%;height:50%;"
          >
    </v-img>
    </swiper-slide>
    <div class="swiper-button-prev" slot="button-prev"></div> 
    <div class="swiper-button-next" slot="button-next"></div>
  </swiper> 
  <br>
</div>
</template>


<script>
import { Swiper, SwiperSlide } from 'vue-awesome-swiper'

export default {
  name: 'SimilarityShow',
  data(){
    return{
      swiper: null,
      SimilarityShowList: [],
      userId: '',
      userNickname: '',
      isShow: false
    }
  },
   props:{
    swiperOption: Object,
  },

  components:{
    Swiper,
    SwiperSlide,
  },

  methods: {
    getSimilarityShow: function() {
      this.$store.dispatch('getSimilarityShow', {userId: this.userId})
      .then(res => {
        this.SimilarityShowList = res.data.items
        if (this.SimilarityShowList.length > 1) {
          this.isShow = true
        }
      })
    },
    getUserInfo: function() {
      this.$store.dispatch('getUserInfo')
      .then((res) => {
        this.userNickname = res.data.nickname
        this.userId = res.data.userId
      })
      .then(() => {
        this.getSimilarityShow() 
      })
    },
    moveShowDetail: function (id) {
      this.$router.push({name: `ShowDetail`, params: { showId: id}})
    }
  },
  created: function() {
    this.getUserInfo()
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
  color: green;
}
.swiper-button-next {
  color: green;
}
</style>