<template>
  <div class='wrap'>
    <mu-row class='paper-row'>
      <mu-col class='paper-elem' width="33" tablet="25" desktop="20" v-for="subject in subjects">
        <router-link :to="{name: 'MovieSubject', params:{id: subject.id}}">
          <mu-paper>
            <div class='paper-border'>
              <div class='paper-img' :style="'background-image: url('+subject.images.large+')'">
              </div>
            </div>
            <div class='paper-text'>
              <p> {{subject.title}} </p>
              <p class="star-box">
                <span class="paper-star" v-if="subject.rating.average > 0">
                  <mu-icon class='star-icon' value="star" v-for="n in ratingStar(subject.rating.average).star"/>
                  <mu-icon class='star-icon' value="star_half" v-for="n in ratingStar(subject.rating.average).half"/>
                  <mu-icon class='star-icon' value="star_border" v-for="n in ratingStar(subject.rating.average).left"/>
                </span>

                <span class='paper-rating'>{{subject.rating.average | fixNum}} <span>
              </p>
            </div>
          </mu-paper>
        </router-link>
      </mu-col>

    </mu-row>
  </div>
</template>
<script>
import { mapState } from 'vuex';
import * as type from './../../store/movies/type';


export default {
  name: 'movieList',
  data() {
    return {
    };
  },
  props: ['tabName'],
  computed: mapState({
    subjects(state) {
      return state.movie.movies[this.tabName].subjects;
    },
  }),
  mounted() {
    this.fetchData();
  },
  beforeUpdate() {
    console.log(`[beforeUpdate], tabName: ${this.tabName}`);
    this.fetchData();
  },
  destroyed() {
    console.log('[destroyed]');
  },
  methods: {
    ratingStar(item) {
      const intNum = Math.round(item);
      const star = Math.floor(intNum / 2);
      const half = intNum % 2;
      const left = 5 - star - half;
      return {
        star,
        half,
        left,
      };
    },

    fetchData() {
      // doing
      if (
        !(this.$store.state.movie.movies[this.tabName].subjects &&
          this.$store.state.movie.movies[this.tabName].subjects.length > 0)
        ) {
        this.$store.dispatch(type.FETCH_MOVIES, { type: this.tabName });
      }
    },
  },
};
</script>
<style lang='css' scoped>
.wrap{
  padding: 6px;
}
.wrap>.paper-row{
  justify-content: flex-start;
  flex-flow: row wrap;
}
.paper-elem{
  padding: 6px;
}
.paper-border{
  position: relative;
  border-radius: 2px 2px 0 0;
  overflow: hidden;
}
.paper-img{
  position: relative;
  width: 100%;
  padding-top:141%;
  background-size: cover;
  -moz-background-size: cover;
}
.paper-img-wrap{
  top:0;
  left: 0;
  position: absolute;
  height: 100%;
  width: 100%;
  border-radius: 2px 2px 0px 0px;
  box-sizing: border-box;
}
.paper-img-wrap>img{
  width: 100%;
}
.paper-text{
  padding-top: 6px;
}
.paper-text>p{
  margin: 0;
  padding: 0 6px;
  font-size: 14px;
  height: 20px;
  line-height: 20px;
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
}
.star-icon{
  font-size: 14px;
  margin-right: -4px;
  float: left;
}
.star-box{
  color: #ff6f00;
}
.paper-star{
  margin-right: 4px;
  font-size: 12px;
}
.paper-rating{
  position: relative;
  top: -2px;
}
</style>
