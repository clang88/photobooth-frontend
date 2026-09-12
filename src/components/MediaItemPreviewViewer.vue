<template>
  <div v-if="isImage(item.processed)" class="full-height full-width relative-position flex flex-center">
    <transition name="fade">
      <q-img
        :key="`${item.id}-${item.revision}`"
        :draggable="false"
        class="full-height full-width absolute-top-left"
        fit="contain"
        loading="eager"
        loading-show-delay="800"
        no-transition
        :src="`/media/preview/${item.id}?${item.revision}`"
      />
    </transition>

    <transition name="fade">
      <div
        v-if="isApplyingLongRunningFilter"
        class="column items-center justify-center absolute-center q-pa-lg glass-effect rounded-borders text-white shadow-10"
        style="backdrop-filter: blur(10px); background: rgba(0, 0, 0, 0.4); z-index: 10"
      >
        <q-spinner-dots size="4em" color="primary" class="q-mb-md" />
        <div class="text-h5 text-weight-bold text-center">{{ $t('Filter is processing...') }}</div>
      </div>
    </transition>
  </div>
  <div v-else-if="isVideo(item.processed)" class="full-height">
    <video
      :draggable="false"
      :src="`/media/preview/${item.id}?${item.revision}`"
      class="rounded-borders full-height full-width"
      muted
      autoplay
      style="object-fit: contain; max-width: 100%; max-height: 100%"
      loop
      playsinline
      controlslist="nofullscreen nodownload noremoteplayback noplaybackrate"
      disablepictureinpicture
    ></video>
  </div>

  <!--eslint-disable-next-line @intlify/vue-i18n/no-raw-text-->
  <div v-else>Element not supported to display.</div>
</template>

<script setup lang="ts">
import type { components } from '@/dto/api'
import { isVideo, isImage } from '@/util/media_is_type'

defineProps<{
  item: components['schemas']['MediaitemPublic']
  isApplyingLongRunningFilter?: boolean
}>()
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
