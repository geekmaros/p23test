<script setup lang="ts">
import { computed } from 'vue'

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

const rating = 4.0
const maxRating = 5
const clampedRating = computed(() => Math.min(Math.max(rating, 0), maxRating))
const ratingLabel = computed(() => `${clampedRating.value.toFixed(1)}/${maxRating.toFixed(1)}`)

const ringRadius = 67
const ringCircumference = 2 * Math.PI * ringRadius
const ringStrokeOffset = computed(
  () => ringCircumference - (clampedRating.value / maxRating) * ringCircumference,
)
const ringGradientId = 'customer-ring-gradient'
const ringMarker = computed(() => {
  const progressAngle = -90 + (clampedRating.value / maxRating) * 360
  const angleInRadians = (progressAngle * Math.PI) / 180
  const center = 75

  return {
    x: center + ringRadius * Math.cos(angleInRadians),
    y: center + ringRadius * Math.sin(angleInRadians),
  }
})

const customerInsight = computed(() => {
  const value = clampedRating.value

  if (value === 5) return 'Ideal Customer'
  if (value >= 4) return 'Promising Customer'
  if (value >= 3) return 'Growing Customer'
  if (value >= 2) return 'Stable Customer'
  if (value >= 1) return 'At-Risk Customer'
  return 'Critical Customer'
})
</script>

<template>
  <aside
    class="relative flex flex-col overflow-hidden bg-[#021717] px-5 py-8 text-white xl:sticky xl:top-0 xl:h-screen xl:pt-[72px]"
  >
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

    <article
      class="mx-auto mt-auto w-full max-w-[243px] rounded-[20px] bg-white px-5 py-5 text-[#34373C]"
    >
      <div class="flex items-center justify-between">
        <div>
          <h3 class="text-sm text-[#34373C] font-medium">Customer metric</h3>
          <p class="text-[10px] text-[#616263]">Overall Insight</p>
        </div>
        <span class="text-xl leading-none text-[#616263]">
          <svg
            width="24"
            height="24"
            viewBox="0 0 24 24"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M7 4L15.3306 10.0405C17.5565 11.6545 17.5565 12.3455 15.3306 13.9595L7 20"
              stroke="#34373C"
              stroke-width="1.5"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>
        </span>
      </div>

      <div class="mt-4 flex flex-col items-center">
        <p class="mb-1 text-[7px] text-[#616263]">{{ ratingLabel }}</p>
        <div class="relative grid size-[150px] place-items-center">
          <div
            class="relative z-10 rounded-full bg-[linear-gradient(to_bottom,#F6F6F6,#38696B)] p-[3px]"
          >
            <img
              src="@/assets/images/user.png"
              alt="user avatar"
              class="size-[108px] rounded-full object-cover bg-[#F9FFFF]"
            />
          </div>

          <svg class="absolute inset-0 h-full w-full" viewBox="0 0 150 150" fill="none">
            <defs>
              <linearGradient
                :id="ringGradientId"
                gradientUnits="objectBoundingBox"
                x1="0"
                y1="0"
                x2="1"
                y2="0"
                gradientTransform="rotate(-45.22 0.5 0.5)"
              >
                <stop offset="21.57%" stop-color="#FD6046" />
                <stop offset="80.46%" stop-color="#FFFFFF" />
                <stop offset="80.46%" stop-color="#F3F3F3" />
              </linearGradient>
            </defs>
            <circle
              cx="75"
              cy="75"
              :r="ringRadius"
              :stroke="`url(#${ringGradientId})`"
              stroke-width="2"
              stroke-linecap="round"
              :stroke-dasharray="ringCircumference"
              :stroke-dashoffset="ringStrokeOffset"
              transform="rotate(-90 75 75)"
            />
            <circle :cx="ringMarker.x" :cy="ringMarker.y" r="4" fill="#FD6046" />
          </svg>
        </div>
        <p class="mt-2 text-[10px] font-medium text-[#616263]">{{ customerInsight }}</p>
      </div>
    </article>
  </aside>
</template>
