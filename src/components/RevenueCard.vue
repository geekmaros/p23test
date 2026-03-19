<script setup lang="ts">
import { computed, onBeforeUnmount, onMounted, ref } from 'vue'

type RevenueRange = 'Weekly' | 'Daily' | 'Monthly'

type RevenueDatum = {
  label: string
  shortLabel?: string
  amount: number
}

const rangeOptions: RevenueRange[] = ['Weekly', 'Daily', 'Monthly']

const revenueData: Record<RevenueRange, RevenueDatum[]> = {
  Weekly: [
    { label: 'Monday', shortLabel: 'Mon', amount: 180 },
    { label: 'Tuesday', shortLabel: 'Tue', amount: 300 },
    { label: 'Wednesday', shortLabel: 'Wed', amount: 145 },
    { label: 'Thursday', shortLabel: 'Thu', amount: 340 },
    { label: 'Friday', shortLabel: 'Fri', amount: 300 },
    { label: 'Saturday', shortLabel: 'Sat', amount: 180 },
    { label: 'Sunday', shortLabel: 'Sun', amount: 145 },
  ],
  Daily: [
    { label: '08 AM', amount: 95 },
    { label: '10 AM', amount: 160 },
    { label: '12 PM', amount: 130 },
    { label: '02 PM', amount: 220 },
    { label: '04 PM', amount: 260 },
    { label: '06 PM', amount: 180 },
    { label: '08 PM', amount: 120 },
  ],
  Monthly: [
    { label: 'Jan', amount: 210 },
    { label: 'Feb', amount: 290 },
    { label: 'Mar', amount: 175 },
    { label: 'Apr', amount: 340 },
    { label: 'May', amount: 315 },
    { label: 'Jun', amount: 225 },
    { label: 'Jul', amount: 190 },
  ],
}

const selectedRange = ref<RevenueRange>('Weekly')
const menuOpen = ref(false)
const cardRef = ref<HTMLElement | null>(null)

const currentData = computed(() => revenueData[selectedRange.value])
const maxAmount = computed(() => Math.max(...currentData.value.map(({ amount }) => amount)))
const selectedIndex = ref(
  currentData.value.findIndex(({ amount }) => amount === maxAmount.value),
)
const hoveredIndex = ref<number | null>(null)

const activeIndex = computed(() => hoveredIndex.value ?? selectedIndex.value)

const ticks = [300, 200, 100]

function barHeight(amount: number) {
  return `${Math.max((amount / 340) * 96, 38)}px`
}

function selectRange(range: RevenueRange) {
  selectedRange.value = range
  selectedIndex.value = revenueData[range].findIndex(
    ({ amount }) => amount === Math.max(...revenueData[range].map((entry) => entry.amount)),
  )
  hoveredIndex.value = null
  menuOpen.value = false
}

function handlePointerEnter(index: number) {
  hoveredIndex.value = index
}

function handlePointerLeave() {
  hoveredIndex.value = null
}

function handleBarClick(index: number) {
  selectedIndex.value = index
  hoveredIndex.value = index
}

function handleDocumentClick(event: MouseEvent) {
  if (!cardRef.value) return

  if (!cardRef.value.contains(event.target as Node)) {
    menuOpen.value = false
  }
}

onMounted(() => {
  document.addEventListener('click', handleDocumentClick)
})

onBeforeUnmount(() => {
  document.removeEventListener('click', handleDocumentClick)
})
</script>

<template>
  <article
    ref="cardRef"
    class="rounded-[20px] bg-white pl-1.5 pr-8 pt-5 pb-4 shadow-[0_14px_34px_rgba(18,24,27,0.08),0_2px_8px_rgba(18,24,27,0.06)]"
  >
    <div class="flex items-start justify-between">
      <h3 class="text-sm leading-none font-medium tracking-[-0.04em] text-[#34373C]">
        Revenue
      </h3>

      <div class="relative ">
        <button
          type="button"
          class="flex  items-center gap-2 rounded-[3px] bg-[#5E5D5D] py-1 px-[12px] text-[10px] leading-none font-semibold text-white"
          @click="menuOpen = !menuOpen"
        >
          <span>{{ selectedRange }}</span>
          <svg
            class="size-3 transition-transform duration-200"
            :class="{ 'rotate-180': menuOpen }"
            viewBox="0 0 16 16"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M3.33301 6L7.99967 10.6667L12.6663 6"
              stroke="currentColor"
              stroke-width="1.8"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>
        </button>

        <div
          v-if="menuOpen"
          class="absolute right-0 z-20 mt-2 w-[132px] rounded-[14px] border border-[#ECECEC] bg-white p-1.5 shadow-[0_14px_30px_rgba(18,24,27,0.12)]"
        >
          <button
            v-for="option in rangeOptions"
            :key="option"
            type="button"
            class="flex w-full rounded-[10px] px-3 py-2 text-left text-[9px] text-[#34373C] transition hover:bg-[#F6F6F6]"
            :class="{ 'bg-[#F3E1FF] font-semibold text-[#34373C]': option === selectedRange }"
            @click="selectRange(option)"
          >
            {{ option }}
          </button>
        </div>
      </div>
    </div>

    <div class="mt-4 flex gap-3">
      <div class="flex h-[118px] flex-col justify-end gap-[20px] pb-[26px] text-[10px] leading-none text-[#66686D]">
        <span v-for="tick in ticks" :key="tick">{{ tick }}$</span>
      </div>

      <div class="flex min-w-0 flex-1 items-end gap-[11px]">
        <div
          v-for="(entry, index) in currentData"
          :key="entry.label"
          class="group relative flex min-w-0 flex-1 flex-col items-center"
          @mouseenter="handlePointerEnter(index)"
          @mouseleave="handlePointerLeave"
        >
          <div class="relative flex h-[118px] w-full items-end justify-center">
            <transition name="tooltip-fade">
              <div
                v-if="activeIndex === index"
                class="absolute left-1/2 -top-3 -translate-x-1/2 rounded-[999px] bg-[#F3D1FA] px-2.5 py-2 text-[10px] leading-none font-medium tracking-[-0.03em] text-[#66686D]"
              >
                ${{ entry.amount }}
              </div>
            </transition>

            <button
              type="button"
              class="w-full max-w-[42px] rounded-[10px] transition-transform duration-200 group-hover:-translate-y-0.5"
              :class="activeIndex === index ? 'bg-[#021717]' : 'bg-[#F5F5F5]'"
              :style="{ height: barHeight(entry.amount) }"
              :aria-label="`${entry.label} revenue $${entry.amount}`"
              @click="handleBarClick(index)"
              @focus="handlePointerEnter(index)"
              @blur="handlePointerLeave"
            />
          </div>

          <span class="mt-3 text-[10px] leading-none font-medium tracking-[-0.03em] text-[#3C4046]">
            {{ entry.shortLabel ?? entry.label }}
          </span>
        </div>
      </div>
    </div>
  </article>
</template>

<style scoped>
.tooltip-fade-enter-active,
.tooltip-fade-leave-active {
  transition: opacity 160ms ease, transform 160ms ease;
}

.tooltip-fade-enter-from,
.tooltip-fade-leave-to {
  opacity: 0;
  transform: translate(-50%, 8px);
}
</style>
