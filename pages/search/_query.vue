<template>
  <div class="container">
    <div class="like">
      <p>{{ query }}</p>
    </div>
    <h3 v-if="data.total === 0">
      По запросу "{{ query }}" 0 результатов
      <br />Попробуйте изменить запрось
    </h3>
    <!-- {{ data.results.length }} -->
    <div class="wrapper">
      <client-only>
        <masonry :cols="{default: 3, 996: 2, 576: 1}" :gutter="5">
          <div v-for="(img, index) in data.results" :key="index" class="card">
            <div class="profile-card">
              <div class="profile-info">
                <img :src="img.user.profile_image.small" alt />
                <span class="name">{{ img.user.name }}</span>
                <span class="username">@{{ img.user.username }}</span>
              </div>
              <div class="action-wrapper">
                <div class="action-img-wrapper">
                  <img src="/img/favorite.svg" alt />
                </div>
                <div class="action-img-wrapper">
                  <img src="/img/maximize.svg" alt />
                </div>
                <div class="action-img-wrapper">
                  <a :href="img.links.download" target="_blank">
                    <img src="/img/download.svg" alt />
                  </a>
                </div>
              </div>
            </div>
            <img class="img" :src="img.urls.small" />
          </div>
          <Observer @intersect="intersected" />
        </masonry>
      </client-only>
    </div>
    <div v-if="loading" class="loading-wrapper">
      <div class="sk-fading-circle">
        <div class="sk-circle1 sk-circle"></div>
        <div class="sk-circle2 sk-circle"></div>
        <div class="sk-circle3 sk-circle"></div>
        <div class="sk-circle4 sk-circle"></div>
        <div class="sk-circle5 sk-circle"></div>
        <div class="sk-circle6 sk-circle"></div>
        <div class="sk-circle7 sk-circle"></div>
        <div class="sk-circle8 sk-circle"></div>
        <div class="sk-circle9 sk-circle"></div>
        <div class="sk-circle10 sk-circle"></div>
        <div class="sk-circle11 sk-circle"></div>
        <div class="sk-circle12 sk-circle"></div>
      </div>
    </div>
  </div>
</template>

<script>
import Observer from '@/components/Observer'

export default {
  components: {
    Observer
  },
  async asyncData (context) {
    const { query } = context.route.params
    const { data } = await context.app
      .$axios({
        method: 'get',
        url: 'https://api.unsplash.com/search/photos',
        headers: {
          Authorization:
            'Client-ID dSdQVrRZY5Gv0FN4l5NH3EijjGeA62o9JeVbDygqTek'
        },
        params: {
          page: 1,
          query,
          per_page: 24
        }
      })
      .catch(() => {})

    return { data, query }
  },
  data () {
    return {
      page: 2,
      observer: null,
      loading: false
    }
  },
  mounted () {
    const options = this.options || {}
    this.observer = new IntersectionObserver(([entry]) => {
      if (entry && entry.isIntersecting) {
        this.$emit('intersect')
      }
    }, options)

    this.observer.observe(this.$el)
  },
  methods: {
    async intersected () {
      if (!this.loading) {
        this.loading = true
        const { data } = await this.$axios({
          method: 'get',
          url: 'https://api.unsplash.com/search/photos',
          headers: {
            Authorization:
            'Client-ID dSdQVrRZY5Gv0FN4l5NH3EijjGeA62o9JeVbDygqTek'
          },
          params: {
            page: this.page,
            query: this.query,
            per_page: 24
          }
        })

        this.page++
        this.data.results = [...this.data.results, ...data.results]
        this.loading = false
      }
    }
  }
}
</script>

<style>

.sk-fading-circle {
  margin: 100px auto;
  width: 40px;
  height: 40px;
  position: relative;
}

.sk-fading-circle .sk-circle {
  width: 100%;
  height: 100%;
  position: absolute;
  left: 0;
  top: 0;
}

.sk-fading-circle .sk-circle:before {
  content: '';
  display: block;
  margin: 0 auto;
  width: 15%;
  height: 15%;
  background-color: #333;
  border-radius: 100%;
  -webkit-animation: sk-circleFadeDelay 1.2s infinite ease-in-out both;
          animation: sk-circleFadeDelay 1.2s infinite ease-in-out both;
}
.sk-fading-circle .sk-circle2 {
  -webkit-transform: rotate(30deg);
      -ms-transform: rotate(30deg);
          transform: rotate(30deg);
}
.sk-fading-circle .sk-circle3 {
  -webkit-transform: rotate(60deg);
      -ms-transform: rotate(60deg);
          transform: rotate(60deg);
}
.sk-fading-circle .sk-circle4 {
  -webkit-transform: rotate(90deg);
      -ms-transform: rotate(90deg);
          transform: rotate(90deg);
}
.sk-fading-circle .sk-circle5 {
  -webkit-transform: rotate(120deg);
      -ms-transform: rotate(120deg);
          transform: rotate(120deg);
}
.sk-fading-circle .sk-circle6 {
  -webkit-transform: rotate(150deg);
      -ms-transform: rotate(150deg);
          transform: rotate(150deg);
}
.sk-fading-circle .sk-circle7 {
  -webkit-transform: rotate(180deg);
      -ms-transform: rotate(180deg);
          transform: rotate(180deg);
}
.sk-fading-circle .sk-circle8 {
  -webkit-transform: rotate(210deg);
      -ms-transform: rotate(210deg);
          transform: rotate(210deg);
}
.sk-fading-circle .sk-circle9 {
  -webkit-transform: rotate(240deg);
      -ms-transform: rotate(240deg);
          transform: rotate(240deg);
}
.sk-fading-circle .sk-circle10 {
  -webkit-transform: rotate(270deg);
      -ms-transform: rotate(270deg);
          transform: rotate(270deg);
}
.sk-fading-circle .sk-circle11 {
  -webkit-transform: rotate(300deg);
      -ms-transform: rotate(300deg);
          transform: rotate(300deg);
}
.sk-fading-circle .sk-circle12 {
  -webkit-transform: rotate(330deg);
      -ms-transform: rotate(330deg);
          transform: rotate(330deg);
}
.sk-fading-circle .sk-circle2:before {
  -webkit-animation-delay: -1.1s;
          animation-delay: -1.1s;
}
.sk-fading-circle .sk-circle3:before {
  -webkit-animation-delay: -1s;
          animation-delay: -1s;
}
.sk-fading-circle .sk-circle4:before {
  -webkit-animation-delay: -0.9s;
          animation-delay: -0.9s;
}
.sk-fading-circle .sk-circle5:before {
  -webkit-animation-delay: -0.8s;
          animation-delay: -0.8s;
}
.sk-fading-circle .sk-circle6:before {
  -webkit-animation-delay: -0.7s;
          animation-delay: -0.7s;
}
.sk-fading-circle .sk-circle7:before {
  -webkit-animation-delay: -0.6s;
          animation-delay: -0.6s;
}
.sk-fading-circle .sk-circle8:before {
  -webkit-animation-delay: -0.5s;
          animation-delay: -0.5s;
}
.sk-fading-circle .sk-circle9:before {
  -webkit-animation-delay: -0.4s;
          animation-delay: -0.4s;
}
.sk-fading-circle .sk-circle10:before {
  -webkit-animation-delay: -0.3s;
          animation-delay: -0.3s;
}
.sk-fading-circle .sk-circle11:before {
  -webkit-animation-delay: -0.2s;
          animation-delay: -0.2s;
}
.sk-fading-circle .sk-circle12:before {
  -webkit-animation-delay: -0.1s;
          animation-delay: -0.1s;
}

@-webkit-keyframes sk-circleFadeDelay {
  0%, 39%, 100% { opacity: 0; }
  40% { opacity: 1; }
}

@keyframes sk-circleFadeDelay {
  0%, 39%, 100% { opacity: 0; }
  40% { opacity: 1; }
}

.card {
  position: relative;
  overflow: hidden;
}

.profile-card {
  transition: opacity 0.3s ease;
  opacity: 0;
  position: absolute;
  top: 0;
  z-index: 10;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-evenly;
  width: 100%;
  height: 100%;
}

.profile-info {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
.profile-info img {
  border-radius: 5px;
  border: 1px solid #fff;
  width: 40px;
  height: 40px;
}

.profile-info span {
  color: #fff;
}
.profile-info .name {
  font-size: 30px;
}
.profile-info .username {
  font-size: 18px;
}

.action-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
}

.action-img-wrapper {
  height: 36px;
  width: 36px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-left: 10px;
  margin-right: 10px;
}

.action-wrapper img {
  /* opacity: 0.5; */
  cursor: pointer;
  height: 22px;
  width: 22px;
  transition: all 0.1s ease;
}

.action-wrapper img:hover {
  width: 36px;
  height: 36px;
}

.like {
  display: flex;
  justify-content: center;
  font-family: SF UI Display;
  font-size: 72px;
  line-height: 86px;
  margin-top: 100px;
  margin-bottom: 100px;
}

.img {
  width: 100%;
}

.card:hover > .profile-card {
  opacity: 1;
}

.card:hover > .img {
  filter: blur(3px);
}

.container {
  margin: 0 auto;
  width: 1100px;
  min-height: 4000px;
}
@media (max-width: 1100px) {
  .like {
    display: none;
  }
  .container {
    width: 700px;
  }
}
@media (max-width: 576px) {
  .like {
    display: none;
  }
  .container {
    width: 400px;
  }
}
</style>
