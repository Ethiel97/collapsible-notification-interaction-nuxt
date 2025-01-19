<script setup lang="ts">
import Tab from '~/components/Tab.vue';
import AcademicCapIcon from '~/components/icons/AcademicCapIcon.vue';
import OfficeIcon from '~/components/icons/OfficeIcon.vue';
import CloudIcon from '~/components/icons/CloudIcon.vue';
import SunIcon from '~/components/icons/SunIcon.vue';
import ChatBubbleOvalLeftIcon from '~/components/icons/ChatBubbleOvalLeftIcon.vue';
import TrophyIcon from '~/components/icons/TrophyIcon.vue';
import ClockIcon from '~/components/icons/ClockIcon.vue';
import NotificationIcon from '~/components/icons/NotificationIcon.vue';
import ChevronUpIcon from '~/components/icons/ChevronUpIcon.vue';
import { Notification } from '~/types.ts';
import { useMotions } from '@vueuse/motion';
import { ref } from 'vue';

const motions = useMotions();
const isCollapsed = ref(true);

const notification = {
  title: '5 new notifications',
  icon: NotificationIcon,
};

const notifications = <Notification>[
  {
    title: 'New message',
    description: 'Ethiel shared the project update',
    date: 'Just now',
    icon: ChatBubbleOvalLeftIcon,
  },
  {
    title: 'Meeting',
    description: 'Team meeting today at the office',
    date: '30 min ago',
    icon: OfficeIcon,
  },
  {
    title: 'Level up',
    description: "You've reached Senior Developer level",
    date: '2 hours ago',
    icon: TrophyIcon,
  },
  {
    title: 'Reminder: Code review',
    description: 'Frontend team code review in 30 min',
    date: '4 days ago',
    icon: ClockIcon,
  },
  {
    title: 'File upload',
    description: 'Your resume has been uploaded',
    date: '7 Jan. 2025',
    icon: CloudIcon,
  },
];

onMounted(() => {
  notifications.forEach((_, index) => {
    //motions[`notification-${index}`]?.apply('collapsed');
  });
});

const toggleNotifications = () => {
  isCollapsed.value = !isCollapsed.value;

  motions[`notification-wrapper`].variant.value = isCollapsed.value
    ? 'collapsed'
    : 'expanded';

  console.log(motions);

  // Apply animation to each notification with staggered delay

  notifications.forEach((_, index) => {
    motions[`notification-${index}`].variant.value = isCollapsed.value
      ? 'collapsed'
      : 'expanded';
  });
};
</script>

<template>
  <div class="bg-white h-screen w-full flex items-center justify-center">
    <div class="px-4 py-3 rounded-xl shadow-md border w-[540px]">
      <div class="flex flex-col overflow-hidden">
        <NotificationItem
          :notification="notification"
          class="my-2"
          :class="
            isCollapsed ? '' : 'border-b border-gray-100 rounded-none pb-2'
          "
        >
          <template #icon>
            <component :is="notification.icon" />
          </template>
          <template #trailing>
            <div
              @click="toggleNotifications"
              class="rounded-full p-1 bg-gray-300 text-white !cursor-pointer"
            >
              <ChevronUpIcon
                :class="
                  (isCollapsed ? 'rotate-180' : '') +
                  ' transition duration-200 ease-in'
                "
              />
            </div>
          </template>
        </NotificationItem>

        <div
          class="overflow-scroll flex flex-col justify-center"
          v-motion="'notification-wrapper'"
          :initial="{ height: 0 }"
          :enter="{
            height: 0,
          }"
          :variants="{
            expanded: {
              height: 400,
              transition: {
                height: {
                  duration: 300,
                },
              },
            },
            collapsed: {
              height: 0,
              transition: {
                height: {
                  duration: 300,
                },
              },
            },
          }"
        >
          <NotificationItem
            styleClass="py-2 my-1"
            v-for="(notification, index) in notifications"
            :key="index"
            :notification="notification"
            v-motion="`notification-${index}`"
            :initial="{ opacity: 0, y: 100 }"
            :variants="{
              expanded: {
                opacity: 1,
                y: 0,
                transition: {
                  duration: 300,
                  delay: index * 50,
                },
              },
              collapsed: {
                opacity: 0,
                y: 100,
                transition: {
                  duration: 300,
                },
              },
            }"
          >
            <template #icon>
              <component :is="notification.icon" />
            </template>
          </NotificationItem>
        </div>
      </div>
    </div>
  </div>
</template>

<!-- :variants="{
  collapsed: {
    opacity: 0,
    y: 20,
    transition: {
      duration: 300,
      delay: `index * 100`,
    },
  },
  expanded: {
    opacity: 1,
    y: 0,
    height: 'auto',
    transition: {
      duration: 300,
      delay: `index*100`,
    },
  },
}" -->
