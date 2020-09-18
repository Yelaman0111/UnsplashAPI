<template>
  <div class="container">
    <div class="like">
      <p>Избранное</p>
    </div>
    <div class="wrapper">
      <client-only>
        <masonry
          :cols="{default: 3, 996: 2, 576: 1}"
          :gutter="25"
        >
          <div v-for="(img, index) in data" :key="index" class="card">
            <div class="profile-card">
              <div class="profile-info">
                <img :src="img.user.profile_image.small" alt />
                <span class="name">{{ img.user.name }}</span>
                <span class="username">{{ img.user.username }}</span>
              </div>
              <div class="action-wrapper">
                <div class="action-img-wrapper">
                  <img src="/img/favorite_24px.svg" alt />
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
            <img
              class="img"
              :src="img.urls.small"
              alt=""
            >
          </div>
        </masonry>
      </client-only>
    </div>
  </div>
</template>

<script>

export default {
  async asyncData (context) {
    const { data } = await context.app
      .$axios({
        method: 'get',
        url: 'https://api.unsplash.com/users/yelaman0111/likes',
        headers: { Authorization: 'Client-ID dSdQVrRZY5Gv0FN4l5NH3EijjGeA62o9JeVbDygqTek' }
      })

      .catch(() => {})

    console.log(data)

    return { data }
  },
  data () {
    return {

      search: ''
    }
  }
}
</script>

<style>
.action-wrapper{
  display: flex;
  justify-content: center;
  align-items: center;
}
.action-img-wrapper{
  width: 36px;
  height: 36px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-left: 10px;
  margin-right: 10px;
}
.action-wrapper img{
  width: 22px;
  height: 22px;
  cursor: pointer;
transition: 0.1s ease;
}
.action-wrapper img:hover{
  width: 36px;
  height: 36px;
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
.card:hover > .profile-card {
  opacity: 1;
}
.card:hover > .img {
  filter: blur(3px);
}
.wrapper {
  display: grid;
        grid-template-columns: auto auto auto;
        grid-template-rows: auto auto auto;
}

.like{
  display: flex;
  justify-content: center;
  font-family: SF UI Display;
  font-size: 72px;
  line-height: 86px;
  margin-top: 100px;
  margin-bottom: 100px;
}
.container {
  min-height: 4000px;
}
 @media (max-width: 576px) {
 .like{
   display: none;
 }
 .wrapper{
  grid-template-columns: 100%;
  grid-gap: 30px;
  margin-top: 50px;
  width: 80%;
  margin: 50px 10% 10px 10%;
}
 }
</style>
