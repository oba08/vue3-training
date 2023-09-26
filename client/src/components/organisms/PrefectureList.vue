<script lang="ts" setup>
import { onMounted, ref } from "vue"
import axiosInstance from "@/utils/axiosSettings"
import { PrefectureDisplay } from "@/types/prefecture"

// const chiba = ref("千葉県")

// TODO: prefectureで千葉県を表示してみましょう
const prefecture = ref<PrefectureDisplay>({
  prefCode: 0,
  prefName: "千葉県",
  isCheck: false
})

// function change() {
//   prefecture.value.isCheck = !prefecture.value.isCheck
// }

// function change(eve: boolean) {
//   prefecture.value.isCheck = eve
// }

onMounted(() => {
  // TODO: 全県取得のAPIへリクエストを送ってみましょう!
  const response = axiosInstance.get("")
})
</script>
<template>
  <div class="prefecture-container">
    <h3>都道府県</h3>
    <!-- <div>{{ chiba }}</div> -->
    <div class="prefecture-flex">
      <!-- TODO: 県を表示してみましょう -->
      {{ prefecture.prefName }}
      {{ prefecture.isCheck }}
      <!-- <input
        type="checkbox"
        @change="prefecture.isCheck = !prefecture.isCheck"
      /> -->
      <!-- <input
        type="checkbox"
        checked="prefecture.isCheck"
        value="prefecture.isCheck"
        @change="change(!prefecture.isCheck)"
      /> -->
      <input v-model="prefecture.isCheck" type="checkbox" />
      <!-- <input type="checkbox" @input="change" /> -->
      <div v-if="prefecture.isCheck">isChecked</div>
    </div>
  </div>
</template>
<style scoped>
.prefecture-container {
  max-width: 900px;
  width: 100%;
}

.prefecture-flex {
  display: grid;
  grid-template-columns: 33% 33% 33%;
}
</style>
