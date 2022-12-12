<script setup>
import anime from "animejs/lib/anime.es.js";
import HelloWorld from "./components/HelloWorld.vue";
import TheWelcome from "./components/TheWelcome.vue";
import { ref, onMounted } from "vue";
const easings = ["easeInOutQuad", "easeInOutCirc", "easeInOutSine", "spring"];
let trianglePoints = null;
function fitElementToParent(el, padding) {
  var timeout = null;
  function resize() {
    if (timeout) clearTimeout(timeout);
    anime.set(el, { scale: 1 });
    var pad = padding || 0;
    var parentEl = el.parentNode;
    var elOffsetWidth = el.offsetWidth - pad;
    var parentOffsetWidth = parentEl.offsetWidth;
    var ratio = parentOffsetWidth / elOffsetWidth;
    timeout = setTimeout(anime.set(el, { scale: ratio }), 10);
  }
  resize();
  window.addEventListener("resize", resize);
}
function createKeyframes(value) {
  var keyframes = [];
  for (var i = 0; i < 30; i++) keyframes.push({ value: value });
  return keyframes;
}
function animateShape(el) {
  var circleEl = el.querySelector("circle");
  var rectEl = el.querySelector("rect");
  var polyEl = el.querySelector("polygon");

  var animation = anime
    .timeline({
      targets: el,
      duration: function () {
        return anime.random(600, 2200);
      },
      easing: function () {
        return easings[anime.random(0, easings.length - 1)];
      },
      complete: function (anim) {
        animateShape(anim.animatables[0].target);
      },
    })
    .add(
      {
        translateX: createKeyframes(function (el) {
          return el.classList.contains("large")
            ? anime.random(-300, 300)
            : anime.random(-520, 520);
        }),
        translateY: createKeyframes(function (el) {
          return el.classList.contains("large")
            ? anime.random(-110, 110)
            : anime.random(-280, 280);
        }),
        rotate: createKeyframes(function () {
          return anime.random(-180, 180);
        }),
      },
      0
    );
  if (circleEl) {
    animation.add(
      {
        targets: circleEl,
        r: createKeyframes(function () {
          return anime.random(32, 72);
        }),
      },
      0
    );
  }
  if (rectEl) {
    animation.add(
      {
        targets: rectEl,
        width: createKeyframes(function () {
          return anime.random(64, 120);
        }),
        height: createKeyframes(function () {
          return anime.random(64, 120);
        }),
      },
      0
    );
  }
  if (polyEl) {
    animation.add(
      {
        targets: polyEl,
        points: createKeyframes(function () {
          var scale = anime.random(72, 180) / 100;
          return trianglePoints
            .map(function (p) {
              return p * scale;
            })
            .join(" ");
        }),
      },
      0
    );
  }
}
onMounted(() => {
  var layeredAnimationEl = document.querySelector(".layered-animations");
  var shapeEls = layeredAnimationEl.querySelectorAll(".shape");
  var triangleEl = layeredAnimationEl.querySelector("polygon");
  trianglePoints = triangleEl.getAttribute("points").split(" ");
  fitElementToParent(layeredAnimationEl);
  for (var i = 0; i < shapeEls.length; i++) {
    animateShape(shapeEls[i]);
  }
});
</script>

<template>
  <header>
    <div class="container mx-auto px-4">
      <div class="content flex justify-center items-center">
        <div class="text">
          <img class="logo" src="./assets/images/logo.png" alt="">
          <div class="main">
            <div class="title">服務發展設計中心</div>
            <div class="title">2023實習計畫</div>
            <div class="sub-text">CHT Design Center Internship program 2023</div>
            <div class="sub-title">一起成為未來造夢師</div>
            <a href="https://forms.gle/FQtiLn7qntmVTHuQA" target="_blank" class="btn apply">立即投遞</a>
          </div>
        </div>
        <img class="person" src="./assets/images/header_person.png" />
      </div>

    </div>
    <div class="haloBallBg haloBall_1"></div>
    <div class="haloBallBg haloBall_2"></div>
    <!-- animation block -->
    <div class="animation-wrapper">
      <div class="layered-animations">
        <svg class="large shape" viewBox="0 0 96 96">
          <defs>
            <linearGradient id="circleGradient" x1="0%" x2="100%" y1="20%" y2="80%">
              <stop stop-color="#373734" offset="0%" />
              <stop stop-color="#242423" offset="50%" />
              <stop stop-color="#0D0D0C" offset="100%" />
            </linearGradient>
          </defs>
          <circle cx="48" cy="48" r="28" fill-rule="evenodd" stroke-linecap="square" fill="url(#circleGradient)" />
        </svg>
        <svg class="small shape color-red" viewBox="0 0 96 96">
          <polygon fill-rule="evenodd" points="48 17.28 86.4 80.11584 9.6 80.11584" stroke-linecap="square" />
        </svg>
        <svg class="large shape" viewBox="0 0 96 96">
          <defs>
            <linearGradient id="triangleGradient" x1="0%" x2="100%" y1="20%" y2="80%">
              <stop stop-color="#373734" offset="0%" />
              <stop stop-color="#242423" offset="50%" />
              <stop stop-color="#0D0D0C" offset="100%" />
            </linearGradient>
          </defs>
          <polygon fill-rule="evenodd" points="48 17.28 86.4 80.11584 9.6 80.11584" stroke-linecap="square"
            fill="url(#triangleGradient)" />
        </svg>
        <svg class="x-small shape" viewBox="0 0 96 96">
          <polygon fill-rule="evenodd" points="48 17.28 86.4 80.11584 9.6 80.11584" stroke-linecap="square" />
        </svg>
        <svg class="x-small shape" viewBox="0 0 96 96">
          <rect width="48" height="48" x="24" y="24" fill-rule="evenodd" stroke-linecap="square" />
        </svg>
        <svg class="small shape color-red" viewBox="0 0 96 96">
          <rect width="48" height="48" x="24" y="24" fill-rule="evenodd" stroke-linecap="square" />
        </svg>
        <svg class="large shape" viewBox="0 0 96 96">
          <defs>
            <linearGradient id="rectGradient" x1="0%" x2="100%" y1="20%" y2="80%">
              <stop stop-color="#373734" offset="0%" />
              <stop stop-color="#242423" offset="50%" />
              <stop stop-color="#0D0D0C" offset="100%" />
            </linearGradient>
          </defs>
          <rect width="48" height="48" x="24" y="24" fill-rule="evenodd" stroke-linecap="square"
            fill="url(#rectGradient)" />
        </svg>
        <svg class="small shape color-red" viewBox="0 0 96 96">
          <circle cx="48" cy="48" r="32" fill-rule="evenodd" stroke-linecap="square" />
        </svg>
        <svg class="x-small shape" viewBox="0 0 96 96">
          <circle cx="48" cy="48" r="32" fill-rule="evenodd" stroke-linecap="square" />
        </svg>
      </div>
    </div>
  </header>
</template>

<style scoped lang="scss">
header {
  width: 100%;
  height: 90vh;
  position: relative;
  color: #fff;

  .haloBallBg {
    background: radial-gradient(95.6% 95.6% at 31.2% 44.8%,
        rgba(64, 175, 175, 0.5) 0%,
        rgba(64, 175, 175, 0) 98.34%);
    filter: blur(100px);
  }

  .haloBall_1 {
    position: absolute;
    width: 662px;
    height: 662px;
    left: -242px;
    top: -331px;
  }

  .haloBall_2 {
    position: absolute;
    width: 414px;
    height: 414px;
    right: -242px;
    top: 190px;
  }

  .content {
    height: 80vh;

    // border: 1px solid red;
    .main {
      min-width: 540px;
      margin-left: 75px;
    }

    .logo {
      margin-bottom: 36px;
    }

    .title {
      font-size: 48px;
      font-weight: bold;
    }

    .sub-text {
      color: #BDBBB7;
      font-size: 24px;
      margin-top: 8px;
    }

    .sub-title {
      color: #30FFFF;
      margin-top: 68px;
    }

    .apply {
      margin-top: 36px;
    }

    .person {
      max-width: 100%;
      height: 100%;
      z-index: 1;
    }
  }

  .animation-wrapper {

    width: 60%;
    // display: flex;
    // align-items: center;
    // justify-content: center;
    // border:2px solid red;
    // left:50%;
    // transform: translateX(-25%);

  }

  .layered-animations {
    // overflow: hidden;
    position: absolute;
    // top: 50%;
    left: 35%;
    top: 30%;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 60%;
    height: 50vh;
    z-index: -1;

    // margin: -275px 0 0 -550px;
    // border: 1px solid #fff;
    .shape {
      position: absolute;
      top: 50%;
      overflow: visible;
      width: 200px;
      height: 200px;
      margin-top: -100px;
      stroke: transparent;
      stroke-width: 1px;
      fill: url(#shapesGradient);
    }
  }

  .layered-animations .small.shape {
    width: 64px;
    height: 64px;
    margin-top: -32px;
    // stroke: red;
    fill: #fff;
  }

  .layered-animations .x-small.shape {
    width: 32px;
    height: 32px;
    margin-top: -16px;
    // stroke: red;
    fill: #30ffff;
  }

  @media (max-width:992px) {
    .animation-wrapper {
      width: 80%;
    }

    .layered-animations {
      top: 0;
      left: 0;
      width: 100%;
    }
  }

  @media (max-width:569px) {

    .layered-animations {
      width: 100%;
    }
  }

  // @media (min-width: 740px) {
  //   .layered-animations .shape {
  //     stroke-width: 0.5px;
  //   }
  // }
}
</style>
