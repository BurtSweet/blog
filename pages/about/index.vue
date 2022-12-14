<script setup lang="ts">
import { themeBackground } from "~/utils/constants";
import config from "~/config";

const useSha = useCorrectSha();
const commitUrl = computed(() => `https://github.com/${config.githubName}/${config.githubRepo}/commit/${useSha.value}`);
const buildTime = ref<string>("Unknown");

const paragraphs = [
  "致我已经逝去的,亲爱的达瓦里氏!"
];

onBeforeMount(async () => {
  buildTime.value = await (await fetch("/timestamp.txt")).text();
});
</script>

<template>
  <div class="about flexc">
    <img :src="themeBackground" alt="bg">
    <div class="flexc paragraphs">
      <p v-for="p,idx in paragraphs" :key="idx">
        {{ p }}
      </p>
    </div>
    <div class="status">
      Last built &lt;<a target="_blank" :href="commitUrl">{{ useSha.substring(0, 8) }}</a>&gt; succeeded at
      <time>{{ buildTime }}</time>
    </div>
  </div>
</template>

<style lang="scss">
@import "assets/style/var";

#default-layout.in-about {
  #header {
    background: rgba($background, 0.1);
    backdrop-filter: blur(1px);
  }

  #body {
    padding-top: 0;

    .about {
      width: 100vw;
      height: calc(100vh - #{$footer-height});
      padding-bottom: $footer-height;
      position: relative;

      img {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        object-fit: cover;
        object-position: right;
        z-index: 1;
      }

      .paragraphs {
        position: relative;
        z-index: 2;
        padding-top: 100px + $header-height;
        justify-content: center;

        p {
          font-size: 16px;
          letter-spacing: 0.8px;
          color: white;
          font-weight: 300;

          &:not(:last-of-type) {
            margin-bottom: 20px;
          }
        }
      }

      .status {
        color: #eee;
        opacity: 0.5;
        font-size: 13px;
        position: absolute;
        bottom: 10px;
        right: 10px;
        z-index: 1;
        transition: $common-transition;

        &:hover {
          opacity: 1;
        }

        time {
          color: #ffb350;
        }
      }
    }
  }

  #footer {
    position: fixed;
    width: 100%;
    left: 0;
    bottom: 20px;
    z-index: 2;

    .middle {
      color: #a1a1a1;
    }

    &:hover {
      .middle {
        color: #e7e7e7;
      }
    }
  }

  #footer,
  #body .about {
    a[href] {
      color: $theme-color;
      transition: $common-transition;

      &:hover {
        color: $theme-color-lighten;
      }
    }
  }
}
</style>
