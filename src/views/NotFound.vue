<template>
  <div class="body bg-base-100">
    <HeadModule/>
    <div class="content">
      <div class="ascii bg-base-200 rounded-box shadow"><pre>
 ___   ___  ________  ___   ___
|\  \ |\  \|\   __  \|\  \ |\  \
\ \  \\_\  \ \  \|\  \ \  \\_\  \
 \ \______  \ \  \\\  \ \______  \
  \|_____|\  \ \  \\\  \|_____|\  \
         \ \__\ \_______\     \ \__\
          \|__|\|_______|      \|__|
      </pre>
      </div>
      <div class="main">
        {{ $t('notFound.title') }}
      </div>
      <div class="divider"></div>
      <button class="btn" @click="$router.push('/')">{{ $t('notFound.back') }}</button>
    </div>
  </div>
</template>

<script lang="ts" setup>
import HeadModule from "@/components/headModule.vue";
</script>

<style scoped>
.body {
  margin: 0;
  padding: 0;
  height: 100vh; /* Set to 100% of viewport height */
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.content {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.ascii {
  padding: 30px 30px 10px 30px;
  border-radius: 8px;
  font-size: 1.0em;
  text-align: left;
}

.main {
  margin-top: 20px;
  font-size: 1.2em;
  text-align: center;
}
</style>
