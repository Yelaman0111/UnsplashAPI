<template>
  <header class="header">
    <div class="header-top">
      <div class="header-block-left">
        <nuxt-link to="/">
          <img src="/img/Vector.svg" alt="" class="logo">
          <span>ImageStock</span>
        </nuxt-link>
      </div>
      <div class="header-block-right">
        <ul>
          <li :class="{displayNone:showNavbar}" @click="showNavbar=true,showHistory=false">
            <nuxt-link to>
              <img src="/img/search.svg" alt=""><span>Поиск</span>
              <span class="line"></span>
            </nuxt-link>
          </li>
          <li>
            <nuxt-link to="/">
              <img src="/img/favorite.svg" alt=""><span>Избранное</span>
              <span class="line"></span>
            </nuxt-link>
          </li>
          <li :class="{displayNone:showHistory}" @click="showHistory=true,showNavbar=false">
            <nuxt-link to>
              <img src="/img/history.svg" alt=""><span>История поиска</span>
              <span class="line"></span>
            </nuxt-link>
          </li>
        </ul>
      </div>
    </div>
    <transition name="slide-fade">
      <div v-if="showNavbar" class="search">
        <div class="input">
          <form @submit.prevent="goSearch">
            <input v-model="search" type="text" placeholder="Поиск">
          </form>
        </div>
        <div class="scroll">
          <div class="history-wrapper">
            <span v-for="index of 10" :key="index">{{ index }}</span>
          </div>
        </div>
      </div>
      <div v-if="showHistory" class="search">
        <div class="input">
          <p>Ваши запросы</p>
        </div>
        <div class="scroll">
          <div class="history-wrapper">
            <span v-for="index of 10" :key="index">{{ index }}</span>
          </div>
        </div>
      </div>
    </transition>
  </header>
</template>

<script>
export default {

  data () {
    return {
      showNavbar: false,
      showHistory: false,
      search: ''
    }
  },
  mounted () {
    window.addEventListener('scroll', this.onScroll)
  },
  beforeDestroy () {
    window.removeEventListener('scroll', this.onScroll)
  },
  methods: {
    onScroll () {
      this.showNavbar = false
      this.showHistory = false
    },
    goSearch () {
      if (this.search.trim()) {
        this.$router.push(`/search/${this.search.trim()}`)
      }
    }
  }
}

</script>

<style>
.historyTitle{
   background-color: #000;
  outline: 0;
  color: #fff;
  display: flex;
  text-align: center;
  font-size: 72px;
  /* border: 0; */
  height: 100px;
  width: 100%;
}
.slide-fade-enter-active {
  transition: transform .3s ease;
}
.slide-fade-leave-active {
  transition: transform .3s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to {
  transform: translateY(-100%);
}

.header-top {
  display: flex;
  align-items: center;
  justify-content: space-around;
  min-height: 80px;
  z-index: 10;
  background-color: #000;
}

.header {
  z-index: 100;
  position: fixed;
  width: 100%;
  background-color: #000;
  flex-direction: column;
  height: 80px;
  display: flex;
  justify-content: space-between;
  align-items: space-around;
}

.search {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: #000;
  min-height: 200px;
}
.input {
  margin-top: 10px;
  display: flex;
  justify-content: center;
  flex-direction: column;
}

.input input {
  background-color: #000;
  outline: 0;
  color: #fff;
  display: flex;
  text-align: center;
  font-size: 72px;
  /* border: 0; */
  height: 100px;
  width: 100%;
  border-image: url("/img/line.svg") 50 round;
  border-left: 0;
  border-right: 0;
  border-top: 0;
}

.input input::placeholder {
  color: #fff;
}
.input p{
  color: #fff;
  font-size: 36px;
  margin-bottom: 40px;
}
.scroll {
  overflow: hidden;
  width: 100%;
  display: flex;
  justify-content: center;
}

.history-wrapper {
  height: 40px;
  width: 880px;
  flex-direction: row;
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  position: relative;
}

.history-wrapper span {
  color: #fff;
  font-size: 18px;
  line-height: 21px;
}

.header-block-right ul {
  list-style: none;
  display: flex;
  justify-content: space-between;
}

.header-block-left a {
  text-decoration: none;
  display: flex;
  align-items: center;
}

.header-block-left a img {
  height: 28px;
  width: 28px;
}

.header-block-left a span {
  font-size: 24px;
  padding-left: 18px;
  color: #fff;
}

.header-block-right ul li a {
  position: relative;
  text-decoration: none;
  display: flex;
  align-items: center;
  overflow: hidden;
  margin-right: 15px;
}

.line {
  position: absolute;
  bottom: 0px;
  right: 0;
  transform: translateX(100%);
  width: 30px;
  height: 3px;
  background-color: #fff;
  transition: transform 0.3s cubic-bezier(0.075, 0.82, 0.165, 1);
}

.header-block-right ul li a:hover > .line {
  transform: translateX(0);
}

.header-block-right ul li img {
  height: 24px;
  height: 24px;
  padding-right: 8px;
}

.header-block-right ul li span {
  color: #fff;
}

 @media (max-width: 576px) {
   .header {
     justify-content: space-between;
   }
   .header-block-left a span {
  display: none;
  }
   .header-block-right ul li span {
  display: none;
  }
  .scroll {
    display: block;
    overflow: auto;
  }
  .input input {
    font-size: 36px;
  }
 }

 .displayNone {
   pointer-events: none;
   opacity: 0;
 }

</style>
