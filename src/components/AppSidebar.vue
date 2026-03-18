<script setup lang="ts">
import attendanceIcon from '@/assets/icons/attendance.svg'
import crmIcon from '@/assets/icons/crm.svg'
import overviewIcon from '@/assets/icons/overview.svg'
import projectIcon from '@/assets/icons/project.svg'
import usersIcon from '@/assets/icons/users.svg'

const primaryNav = [
  { label: 'Overview', icon: overviewIcon, active: true },
  { label: 'CRM', icon: crmIcon, active: false },
]

const secondaryNav = [
  { label: 'Project', icon: projectIcon },
  { label: 'Attendance', icon: attendanceIcon },
  { label: 'Users', icon: usersIcon },
]
</script>

<template>
  <aside class="relative overflow-hidden bg-[#021717] px-5 py-8 text-white xl:py-[72px]">
    <div
      class="mx-auto mb-10 flex items-center max-w-[160px] gap-2 xl:mx-0 xl:mb-[52px] xl:ml-[52px]"
    >
      <span class="size-[27px] rounded-full bg-white" />
      <span class="font-['Righteous'] text-2xl">Logo</span>
    </div>

    <nav class="grid grid-cols-2 gap-2 xl:flex xl:flex-col xl:gap-[18px]">
      <div class="top-nav-links pb-[17px] border-b border-[#6E8788]">
        <router-link
          v-for="item in primaryNav"
          :key="item.label"
          :class="[
            'mb-5 flex items-center gap-3 rounded-[20px] px-5 py-[11px] text-base font-medium transition-colors duration-200 last:mb-0',
            !item.active && 'hover:bg-white/12',
            item.active ? 'bg-[#356364]' : 'bg-transparent',
          ]"
          to="#"
        >
          <img :src="item.icon" :alt="`${item.label} icon`" class="size-6" />
          <span>{{ item.label }}</span>
        </router-link>
      </div>

      <router-link
        v-for="item in secondaryNav"
        :key="item.label"
        class="flex items-center gap-3 rounded-[20px] px-5 py-[11px] text-xs font-medium transition-colors duration-200 hover:bg-white/12 xl:ml-8"
        to="#"
      >
        <img :src="item.icon" :alt="`${item.label} icon`" class="size-5" />
        <span>{{ item.label }}</span>
      </router-link>
    </nav>
  </aside>
</template>
