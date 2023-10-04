<script lang="ts" setup>
import { computed, onMounted, ref } from "vue"
import axiosInstance from "@/utils/axiosSettings"
import { PrefectureDisplay } from "@/types/prefecture"
import { PrefectureResponse } from "@/types/api"
import PrefectureCheckVue from "@/components/organisms/PrefectureCheck.vue"

// TODO: prefectureで千葉県を表示してみましょう

// const prefecture = ref<PrefectureDisplay>({
//   prefCode: 0,
//   prefName: "千葉県",
//   isCheck: false
// })

const prefectureListDisplay = ref<PrefectureDisplay[]>([])
const isCheckedPrefectureList = computed(() =>
  prefectureListDisplay.value.filter((x) => x.isCheck)
)

function handleCheck(pref: PrefectureDisplay) {
  const index = prefectureListDisplay.value.findIndex(
    (x) => x.prefCode === pref.prefCode
  )
  prefectureListDisplay.value[index].isCheck = pref.isCheck
}

// const isCheckedPrefectureList2 = computed({
//   get: () => prefectureListDisplay.value.filter((x) => x.isCheck),
//   set: () => prefectureListDisplay.value
// })

onMounted(async () => {
  // TODO: 全県取得のAPIへリクエストを送ってみましょう!
  const response = await axiosInstance.get<PrefectureResponse>("/prefectures")
  prefectureListDisplay.value = response.data.result.map((x) => {
    return {
      ...x,
      isCheck: false
    }
  })
})
</script>
<template>
  <div class="prefecture-container">
    <h3>都道府県</h3>
    <div class="prefecture-flex">
      <div v-for="pref in prefectureListDisplay" :key="pref.prefCode">
        <!-- <div class="prefecture-flex"> -->
        <div>
          <!-- TODO: 県を表示してみましょう -->
          <PrefectureCheckVue
            v-if="prefectureListDisplay.length"
            :prefecture="pref"
            @check="handleCheck"
          ></PrefectureCheckVue>
          {{ pref.isCheck }}
        </div>
      </div>
    </div>
    <!-- <div v-for="pref in prefectureListDisplay" :key="pref.prefCode">
      <div>
        <input v-model="pref.isCheck" type="checkbox" />
        {{ pref.prefName }}
        {{ pref.isCheck }}
        <span v-if="pref.isCheck">isChecked</span>
      </div>
    </div> -->
    <div>
      チェックされた都道府県
      <div
        v-for="isCheckedPref in isCheckedPrefectureList"
        :key="isCheckedPref.prefCode"
      >
        <div class="prefecture-flex">・{{ isCheckedPref.prefName }}</div>
      </div>
    </div>
    <!-- <div>
      チェックされた都道府県(get,set)
      <div
        v-for="isCheckedPref in isCheckedPrefectureList2"
        :key="isCheckedPref.prefCode"
      >
        <div class="prefecture-flex">・{{ isCheckedPref.prefName }}</div>
      </div>
    </div> -->
    <div>
      子コンポーネント
      <PrefectureCheckVue
        v-if="prefectureListDisplay.length"
        :prefecture="prefectureListDisplay[0]"
      />
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
