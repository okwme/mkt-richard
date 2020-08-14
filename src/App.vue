<template>
  <div id="app">
    <home :class="{showNav:showNav}" @closeNav="closeNav" :lang="lang" />
    <div id="nextButton" @click="$refs.fullpage.api.moveSectionDown()"></div>
    <full-page ref="fullpage" :options="options" id="fullpage">
      <!-- page sections -->
      <div class="section"
      v-for="(section, i) in doc.sections"
      :key="i"
      :id="'section' + i"
      :class="{'video-el' : section.type=== 'video'}"
      >
        <!-- (video bg) -->
        <template v-if="section.type === 'video'">
          <video class="fp-video fp-bg" :src="section.video_lnd" autoplay muted loop playsinline></video>
        </template>
        <!-- (image bg) -->
        <template v-else-if="section.type === 'image'">
          <div role="img" class="fp-bg" :style="`background-image:url(${section.image})`" :aria-label="section.alt"></div>
        </template>
        <!-- (landing image) -->
        <template v-if="i === 0 && doc.landing_img">
          <div class="landingImage" @click="goToNews">
            <img :src="doc.landing_img" :alt="doc.landing_img_alt">
          </div>
        </template>
      </div>
      <!-- <div class="section"
      v-for="(content,i) in contents"
      :key="i"
      :id="'section' + i"
      :class="{mirror:content.name === 'richard_orange.mov' || content.name === '_DSF2502-Bearbeitet.jpg', 'video-el' : content.type=== 'video'}"
      >
        <video
        :autoplay="isMobile"
        playsinline
        preload="metadata"
         v-if="content.type === 'video'" loop muted class="fp-video fp-bg" :poster="content.poster">
          <source
          v-for="(src,i) in content.src"
          :key="i"
          :id="'source'+i"
          :src="content.path + src.file" type="video/mp4" :media="src.media"/>
        </video>
        <div v-else class="fp-bg" :style="'background-image:url(' + content.path + content.name + ')'"></div>
        <div v-if="i === 0 && landingImage" @click="goToNews" class="landingImage"  v-html="landingImage"/>
      </div> -->
    </full-page>
    <div :class="{showNav:showNav, floatLogo: true}"><a href="/"><img src="/static/img/logo_big.png"></a></div>
    <div class="siteLinks">
      <div id="reserve"><a target="_blank" href="https://bookings.seatris.com/restaurants/richard">{{lang === 'en' ? 'booking' : 'reservierung'}}</a></div>
      <div @click="switchLang" id="lang">{{notLang}}</div>
    </div>
    <router-link class="siteNav" @click.native="clickMenu"  to="nav">
      <div id="hamburger"><div class="hb"></div><div class="hb"></div><div class="hb"></div></div>
      <div id="site-menu">Menu</div>
    </router-link>
  </div>
</template>

<script>
import Home from '@/components/Home'
import { isMobile } from './assets/helpers'
import enableInlineVideo from 'iphone-inline-video'

const doc = require('../static/content/home.json')

let contents = [
  {
    type: 'video',
    poster: '/static/images/richard_paul.jpg',
    src: [
      {
        file: 'Richard_Paul 9rf23.00.mp4',
        media: 'all and (max-device-width: 375px)'
      },
      {
        file: 'Richard_Paul 9rf22.00.mp4',
        media: 'all and (max-device-width: 768px)'
      },
      {
        file: 'Richard_Paul 9rf21.00.mp4',
        media: 'all and (max-device-width: 1024px)'
      },
      {
        file: 'Richard_Paul 9rf20.00.mp4',
        media: 'all and (max-device-width: 1025px)'
      }
    ],
    path: '/static/videos/compressed/'
  },
  {
    type: 'image',
    name: 'richard_lamps_image.jpg',
    path: '/static/images/'
  },
  {
    type: 'video',
    poster: '/static/images/richard_door.jpg',
    src: [
      {
        file: 'Richard_Door 4rf23.00.mp4',
        media: 'all and (max-device-width: 375px)'
      },
      {
        file: 'Richard_Door 4rf22.00.mp4',
        media: 'all and (max-device-width: 768px)'
      },
      {
        file: 'Richard_Door 4rf21.00.mp4',
        media: 'all and (max-device-width: 1024px)'
      },
      {
        file: 'Richard_Door 4rf20.00.mp4',
        media: 'all and (max-device-width: 1025px)'
      }
    ],
    path: '/static/videos/compressed/'
  },
  {
    type: 'image',
    name: '54_Wx1_Richard_c_LenaGanssmann-DSC_9460.jpg',
    path: '/static/images/'
  },
  {
    type: 'image',
    name: '75_Wx1_Richard_c_LenaGanssmann-DSC_9661.jpg',
    path: '/static/images/'
  },
  {
    type: 'video',
    poster: '/static/images/richard_celery.jpg',
    src: [
      {
        file: 'Richard_Celery 3rf23.00.mp4',
        media: 'all and (max-device-width: 375px)'
      },
      {
        file: 'Richard_Celery 3rf22.00.mp4',
        media: 'all and (max-device-width: 768px)'
      },
      {
        file: 'Richard_Celery 3rf21.00.mp4',
        media: 'all and (max-device-width: 1024px)'
      },
      {
        file: 'Richard_Celery 3rf20.00.mp4',
        media: 'all and (max-device-width: 1025px)'
      }
    ],
    path: '/static/videos/compressed/'
  },
  {
    type: 'image',
    name: 'RICHARD-Interior.jpg',
    path: '/static/images/'
  },
  {
    type: 'video',
    poster: '/static/images/richard_eggplant.jpg',
    src: [
      {
        file: 'Richard_Eggplant 1rf23.00.mp4',
        media: 'all and (max-device-width: 375px)'
      },
      {
        file: 'Richard_Eggplant 1rf22.00.mp4',
        media: 'all and (max-device-width: 768px)'
      },
      {
        file: 'Richard_Eggplant 1rf21.00.mp4',
        media: 'all and (max-device-width: 1024px)'
      },
      {
        file: 'Richard_Eggplant 1rf20.00.mp4',
        media: 'all and (max-device-width: 1025px)'
      }
    ],
    path: '/static/videos/compressed/'
  },
  {
    type: 'video',
    poster: '/static/images/richard_orange.jpg',
    src: [
      {
        file: 'richard_orangerf23.00.mp4',
        media: 'all and (max-device-width: 375px)'
      },
      {
        file: 'Richard_orangerf22.00.mp4',
        media: 'all and (max-device-width: 768px)'
      },
      {
        file: 'Richard_orangerf21.00.mp4',
        media: 'all and (max-device-width: 1024px)'
      },
      {
        file: 'Richard_orangerf20.00.mp4',
        media: 'all and (max-device-width: 1025px)'
      }
    ],
    path: '/static/videos/compressed/'
  },
  {
    type: 'image',
    name: '_DSF2502-Bearbeitet.jpg',
    path: '/static/images/'
  },
  {
    type: 'image',
    name: 'RR-WEBSITE-BolkBG.jpg',
    path: '/static/images/'
  }
]
// loaded in index.html via <script>
// require('./assets/fullpage.parallax.min.js')
export default {
  name: 'App',
  data () {
    return {
      doc: doc,
      isMobile: isMobile(),
      lang: 'de',
      showNav: false,
      contents,
      options: {
        scrollBar: true,
        loopBottom: true,
        touchSensitivity: 1,
        css3: true,
        onLeave: this.onLeave,
        afterLoad: this.afterLoad,
        normalScrollElements: '#nav',
        licenseKey: 'AF5DCFEF-E50143C7-AE3BDEDE-AF93A31F',
        // licenseKey: '8D262CAD-3210481C-BB3740F8-DB065455',
        parallaxKey: 'cmVzdGF1cmFudC1yaWNoYXJkLmRlX1NPWWNHRnlZV3hzWVhnPXI2dg==',
        // parallaxKey: 'bmV0bGlmeS5jb21fU2JRY0dGeVlXeHNZWGc9bmJV',
        parallax: true,
        parallaxOptions: { type: 'cover', property: 'translate' }
      }
    }
  },
  computed: {
    notLang () {
      return this.lang === 'de' ? 'en' : 'de'
    },
    landingImage () {
      return require('./assets/copy/landing_image.md')
    }
  },
  mounted () {
    if (this.$route.name !== 'richard') this.showNav = true
    let foo = document.querySelector(
      '[href="http://alvarotrigo.com/fullPage/extensions/"]'
    )
    if (foo) {
      foo.parentElement.style.display = 'none !important'
    }
    /* enable inline video on ios */
    document.querySelectorAll('.fp-video').forEach(v => enableInlineVideo(v))
  },
  methods: {
    goToNews () {
      this.showNav = true
      this.$router.push('/news')
    },
    afterLoad (origin, destination, direction) {
      if (!origin) {
        this.onLeave(origin, destination, direction)
        return
      }
      let el = origin.item
      if (hasClass(el, 'video-el')) {
        let vids = el.querySelectorAll('video')
        if (vids.length) {
          vids[0].pause()
        }
      }
    },
    onLeave (origin, destination, direction) {
      let el = destination.item
      if (hasClass(el, 'video-el')) {
        let vids = el.querySelectorAll('video')
        if (vids.length) {
          vids[0].play().catch(error => {
            console.log(error.message)
            vids[0].play()
          })
        }
      }
    },
    switchLang () {
      this.lang = this.lang === 'en' ? 'de' : 'en'
    },
    bgImage (i) {
      return {
        // 'background-image': 'url(https://dummyimage.com/600x400/' + Math.floor(Math.random() * 16777215).toString(16) + '/f00)'
        'background-image':
          'url(http://lorempixel.com/400/200/sports/' + i + ')'
      }
    },
    closeNav () {
      this.$refs.fullpage.api.moveSectionDown()
      this.showNav = false
    },
    clickMenu () {
      this.$refs.fullpage.api.moveSectionDown()
      this.showNav = true
    }
  },
  components: { Home }
}
function hasClass (element, className) {
  if (element.classList) return element.classList.contains(className)
  else {
    return new RegExp('(^| )' + className + '( |$)', 'gi').test(
      element.className
    )
  }
}
</script>

<style lang="scss">
@import 'sanitize.css/sanitize.css';
@import './assets/style/fonts.css';
html,
body {
  padding: 0;
  margin: 0;
  color: black;
  font-family: 'Bodoni', times;
  font-weight: 400;
  font-size: 16px; //21px;
  letter-spacing: 0.06em;
  line-height: 1.6em;
}
h1,
h2,
h3,
h4,
h5,
h6 {
  font-size: 1.35rem;
  font-weight: 600;
  text-align: center;
  margin-top: 2rem;
  margin-bottom: 4rem;
  letter-spacing: 0.06em;
  line-height: 1.2em;
}
a,
a:active,
a:hover,
a:visited {
  text-decoration: none;
  color: black;
}
strong {
  font-weight: 600;
}
* {
  box-sizing: border-box;
}
.section {
  overflow: hidden;
  position: relative;
}
.siteNav,
.siteLinks {
  position: fixed;
  top: 0px;
  display: flex;
  flex-flow: row nowrap;
}
.siteLinks {
  right: 10vw;
}
.siteNav {
  left: 20px;
}
#hamburger {
  background-color: white;
  padding: 8px 8px 9px;

  .hb {
    width: 15px;
    height: 2px;
    display: block;
    background-color: black;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  .hb:not(:last-child) {
    margin-bottom: 3px;
  }
}
#site-menu,
#lang,
#reserve {
  z-index: 2;
  padding: 6px 10px;
  line-height: 1;
  text-transform: uppercase;
}
#lang {
  background-color: black;
  color: white;
  cursor: pointer;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
#site-menu,
#reserve {
  background-color: white;
  color: black;
}
#site-menu {
  padding-left: 0;
}
#nextButton {
  position: fixed;
  left: 50%;
  bottom: 50px;
  width: 40px;
  height: 40px;
  border-right: 3px solid white;
  border-bottom: 3px solid white;
  transform: rotate(45deg) translateX(-50%);
  z-index: 2;
  cursor: pointer;
}
.landingImage {
  position: absolute;
  z-index:10;
  top:10vh;
  left:20px;
  width: calc(100% - 20px);
  height: calc(100% - 30vh);
  cursor: pointer;
  img {
    position:absolute;
    top:0; left:0;
    width:100%; height: 100%;
    object-fit: contain;
    object-position: center;
  }
}
.floatLogo {
  color: white;
  text-align: center;
  position: fixed;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  margin-top: 0;
  margin: auto;
  z-index: 100;
  transition: transform 400ms ease;

  &.showNav {
    transform: translate(-50%, -100%);
  }
}
.floatLogo img {
  background-color: black;
  display: inline-block;
  padding: 7px 10px;
  height: calc(1rem + 14px);
  width: auto;
}

/*.bg-img {
  width: 100%;
  height: 100vh;
  background-position: center center;
  background-repeat: no-repeat;
  background-size: contain;
}*/
/*.fp-bg:before{
  content: '';
  position: absolute;
  display: block;
  background: rgba(173, 173, 173, 0.2);
  top:0;
  bottom:0;
  height: 100%;
  width: 100%;
}*/
.fp-bg {
  // transition: all 1000ms ease;
  height: 100vh;
  background-size: cover;
  background-position: center center;
  background-repeat: no-repeat;
  transform: translate3d(0, 0, 0);
  -webkit-transform: translate3d(0, 0, 0);
  backface-visibility: hidden;
  -webkit-backface-visibility: hidden;
}
video {
  position: absolute;
  right: 0;
  bottom: 0;
  top: 0;
  right: 0;
  width: 100%;
  height: 100%;
  background-size: 100% 100%;
  background-color: black; /* in case the video doesn't fit the whole page*/

  background-position: center center;
  background-size: contain;
  object-fit: cover; /*cover video background */
  z-index: 3;
}
.mirror {
  transform: rotateY(180deg);
}
#section0 .layer {
  position: absolute;
  z-index: 4;
  width: 100%;
  left: 0;
  top: 43%;

  /*
  * Preventing flicker on some browsers
  * See http://stackoverflow.com/a/36671466/1081396  or issue #183
  */
  -webkit-transform: translate3d(0, 0, 0);
  -ms-transform: translate3d(0, 0, 0);
  transform: translate3d(0, 0, 0);
}

@media only all and (max-width: 480px) {
  .logo img {
    max-width: 106px !important;
  }
  .siteLinks {
    right: 0;
  }
  .siteNav {
    left: 0;
  }
  .floatLogo {
    color: white;
    text-align: left;
    position: fixed;
    top: 0;
    left: 2rem;
    transform: none;
    z-index: 1;

    &.showNav {
      transform: none;
    }
  }
  .floatLogo img {
    padding: 7px 10px;
    height: calc(1rem + 13px);
  }
  #lang,
  #reserve {
    font-size: 12px;
    padding: 8px 10px;
  }
  #site-menu {
    display: none;
  }
}
</style>
