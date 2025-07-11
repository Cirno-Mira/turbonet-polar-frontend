<script setup xmlns="http://www.w3.org/1999/html">
import TurboWarningBadge from "@/components/TurboWarningBadge.vue";
import TurboPermissionBadge from "@/components/TurboPermissionBadge.vue";
import {computed, onMounted, ref, watch} from "vue";
import {sendGetRequest} from "@/assets/js/RequestHandler.js";
import SingleFriend from "@/layouts/friends/SingleFriend.vue";
import FriendSettingDialog from "@/layouts/friends/FriendSettingDialog.vue";
import {openDialogModal} from "@/assets/js/DialogManager.js";
import { useI18n } from 'vue-i18n';

const { t } = useI18n()
const isLoading = ref(true);
const isSuccess = ref(false)
const isLoadingFriends = ref(true)
const isLoadingRivals = ref(true)
const isSuccessFriends = ref(false)
const isSuccessRivals = ref(false)
const page = ref(1)

const responseData = ref('')
const rivals = ref({
  turboNameList: []
})

const showFriends = async () => {
  await sendGetRequest('/web/showFriends?page=' + page.value, true).then((response) => {
    if (response.statusCode === 200) {
      isLoadingFriends.value = false
      isSuccessFriends.value = true
      responseData.value = response.data
    } else {
      isLoadingFriends.value = false
      isSuccessFriends.value = false
      responseData.value = response.data
    }
  }).catch(() => {
    isLoadingFriends.value = false
    isSuccessFriends.value = false
    responseData.value = t('error.jsError')
  })
}

const showRival = async () => {
  await sendGetRequest('/web/showRival', true).then((response) => {
    if (response.statusCode === 200) {
      isLoadingRivals.value = false
      isSuccessRivals.value = true
      rivals.value = response.data
    } else {
      isLoadingRivals.value = false
      isSuccessRivals.value = false
      responseData.value = response.data
    }
  }).catch(() => {
    isLoadingRivals.value = false
    isSuccessRivals.value = false
    responseData.value = t('error.jsError')
  })
}

onMounted(() => {
  showFriends()
  showRival()
})

watch(
    [isLoadingFriends, isLoadingRivals],
    ([friendsLoading, rivalsLoading]) => {
      if (!friendsLoading && !rivalsLoading) {
        isLoading.value = false
        isSuccess.value = isSuccessFriends.value && isSuccessRivals.value
      }
    }
)

const friendSetting = () => {
  openDialogModal('friendSettingDialog')
}

</script>

<template>
  <div v-if="isLoading || !isSuccess" class="main-container-center">
    <span v-if="isLoading" class="loading loading-spinner size-8"/>
    <div v-if="!isLoading && !isSuccess">
      <h1 class="font-bold text-3xl">
        <i class="fa-regular fa-circle-xmark"></i> {{  $t }}
      </h1>
      <div class="mt-3"></div>
      <p>
        {{ responseData }}
        <router-link class="text-primary" to="/">{{ $t('error.back') }}</router-link>
      </p>
    </div>
  </div>
  <div v-else>
    <div class="breadcrumbs text-sm">
      <ul>
        <i class="fa-solid fa-link"></i>&nbsp;&nbsp;
        <li>{{ $t('menu.friends.title') }}</li>
        <li>{{ $t('menu.friends.friends') }}</li>
      </ul>
    </div>
    <div class="mt-2"/>
    <div class="flex w-full mx-auto indicator">
      <span @click="friendSetting" class="absolute btn gap-1.5" style="inset-inline-end: 0; border-bottom-right-radius: unset !important; border-top-left-radius: unset !important;">
        <i class="fa-solid fa-gear"></i> {{ $t('friends.setting') }}
      </span>
      <div class="flex border-2 border-base-200 py-4 rounded-box flex-col w-full">
        <div class="flex flex-col place-items-center">
          <div class="stat-title">{{ $t('friends.friendCount') }}</div>
          <div class="stat-value">{{ responseData.totalElements }}/50</div>
        </div>
      </div>
    </div>
    <div class="mt-2"/>
    <div class="text-center my-10" v-if="responseData.totalElements === 0">
      {{ $t('friends.noData') }}
    </div>
    <SingleFriend :friends="responseData.content" :rivals="rivals"/>
  </div>
  <FriendSettingDialog/>
</template>

<style scoped>

</style>