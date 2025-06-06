<script setup>
import axios from "axios";
import { debounce } from "throttle-debounce";
import { watch, ref } from "vue";
const keyword = ref("");
const packages = ref({});

// https://api.cdnjs.com/libraries

watch(keyword, (value) => {
  console.log(value);
  if (value) {
    const endPoint = new URL("https://api.cdnjs.com/libraries");
    endPoint.searchParams.set("search", value);
    endPoint.searchParams.set("limit", 10);

    console.log("go");
    const resp = axios.get(endPoint.toString());
    packages.value = resp.data.results;


    // 測試debounce
    const searchCdn = debounce(
      1000,
      (keyword) => {
        console.log("num:", keywork);
      },
      { atBegin: false }
    );
  }
});
</script>

<template>
  <div>
    <input type="text" v-model="keyword" /><button>搜尋</button>
    <ul>
      <li v-for="pac in packages">
        <a :href="pac.latest" target="_black">pac.name</a>
        {{ pac.name }}
        {{ pac.latest }}
      </li>
    </ul>
  </div>
</template>

<style scoped></style>
