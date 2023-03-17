<script setup lang="ts">
import { computed, reactive, ref } from "vue";
import { useFetch } from "@vueuse/core";

document.title = "Stable Diffusion WebUI Extensions";
const url = ref("https://raw.githubusercontent.com/AUTOMATIC1111/stable-diffusion-webui-extensions/master/index.json");
// const url = ref("/extensions.json");
const refetch = ref(false);
const { data, error, statusCode, isFetching, isFinished, canAbort } = useFetch(
  url,
  { refetch }
).get();
const text = reactive({
  isFinished,
  isFetching,
  canAbort,
  statusCode,
  error,
  data: computed(() => {
    try {
      return JSON.parse(data.value as string);
    } catch (e) {
      return null;
    }
  }),
});
</script>

<template>
  <div class="creator-archive">
    <span style="font-size: 40px; color: #E6E6E6;">Stable Diffusion WebUI Extensions</span>
    <div>
    <a href="https://github.com/camenduru/stable-diffusion-webui-extensions"><img alt="GitHub" src="/github.svg"  height="20" /></a>&nbsp;
      <a href="https://twitter.com/camenduru"><img alt="Twitter" src="/twitter.svg" height="20" /></a>&nbsp;
      <a href="https://discord.gg/k5BwmmvJJU"><img alt="Discord" src="/discord.svg" height="20" /></a>&nbsp;
    </div>
    <span style="font-size: 18px; color: hotpink;"> Forked from </span>
  <img src="/logo.png" width="22" height="22" />
  <span style="font-size: 20px; color: #E6E6E6;"> AI</span><span style="font-size: 20px; color: hotpink;">CA </span>
  <a href="https://github.com/camenduru/aica"><span style="font-size: 12px; color: whitesmoke;"> AI
    </span>
    <span style="font-size: 12px; color: hotpink;">Creator Archive</span></a>
    <div>
      <span style="font-size: 22px; color: whitesmoke;">🚨 WebUI extension developers, please add your extension to the index</span><br>
      <a href="https://github.com/AUTOMATIC1111/stable-diffusion-webui-extensions"><span style="font-size: 22px; color: hotpink;">https://github.com/AUTOMATIC1111/stable-diffusion-webui-extensions</span></a>
    </div>
    <br>
  </div>
  <masonry-wall v-if="text.data.extensions" :items="text.data.extensions" :ssr-columns="1" :column-width="350" :gap="10">
    <template #default="{ item, index }">
      <div class="grid-item">
        <a :href="`${item.url}`"><span style="font-size: 22px; color: hotpink;">{{ item.name }}</span></a><br>
        <span v-for="(tag, tag_index) in item.tags" :key="tag_index">
          <span style="font-size: 16px; color: hotpink;">{{ tag }}</span>&nbsp;
        </span>
        <br>
        <span style="font-size: 22px; color: whitesmoke;">{{ item.description }}</span><br>
        <span style="font-size: 12px; color: hotpink;">{{ item.added }}</span><br>
        <a :href="`${item.url}`"><span style="font-size: 22px; color: hotpink;">README</span></a><br>
      </div>
    </template>
  </masonry-wall>
</template>
