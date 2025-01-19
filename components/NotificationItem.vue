<template>
  <div
    @click="$emit('tabClicked', label)"
    class="rounded-lg flex items-center gap-4"
    :class="styleClass"
  >
    <div
      class="p-3 flex items-center justify-center bg-gray-100 text-gray-400 rounded-lg"
    >
      <slot name="icon">
        {{ notification.icon }}
      </slot>
    </div>

    <div class="flex flex-col justify-start gap-2 grow">
      <h5 class="text-gray-800">{{ notification.title }}</h5>
      <p
        v-if="!!notification.description"
        class="text-gray-400 line-clamp-1 ellipsis w-5/6"
      >
        {{ notification.description }}
      </p>
    </div>

    <slot name="trailing">
      <span v-if="!!notification?.date" class="text-gray-400">
        {{ notification.date }}
      </span>
    </slot>
  </div>
</template>

<script lang="ts" setup>
import { Notification } from '~/types.ts';

const props = defineProps({
  notification: {
    type: Object as PropType<Notification>,
  },
  styleClass:{
    type: String,
  }
});
</script>
