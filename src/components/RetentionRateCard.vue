<script setup lang="ts">
import { computed, onBeforeUnmount, onMounted, ref } from 'vue'

type RetentionRange = 'Weekly' | 'Daily' | 'Monthly'

type RetentionDatum = {
  value: number
}

const rangeOptions: RetentionRange[] = ['Weekly', 'Daily', 'Monthly']

const retentionData: Record<RetentionRange, RetentionDatum> = {
  Weekly: { value: 58 },
  Daily: { value: 46 },
  Monthly: { value: 78 },
}

const selectedRange = ref<RetentionRange>('Weekly')
const menuOpen = ref(false)
const cardRef = ref<HTMLElement | null>(null)

const currentValue = computed(() => retentionData[selectedRange.value].value)
const pointerAngle = computed(() => 180 - (currentValue.value / 100) * 180)
const indicatorStyle = computed(() => {
  const radians = (pointerAngle.value * Math.PI) / 180
  const centerX = 100
  const centerY = 93
  const midRadius = 69
  const length = 58

  const x = centerX + midRadius * Math.cos(radians)
  const y = centerY - midRadius * Math.sin(radians)

  return {
    left: `${x - length / 2}px`,
    top: `${y - 2}px`,
    transform: `rotate(${-pointerAngle.value}deg)`,
  }
})

function selectRange(range: RetentionRange) {
  selectedRange.value = range
  menuOpen.value = false
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
    class="rounded-[20px] bg-[#F9FFFF] px-5 pt-5 pb-4 shadow-[0_1px_7px_rgba(12,12,13,0.1)]"
  >
    <div class="flex items-start justify-between">
      <h3 class="text-sm leading-none font-medium tracking-[-0.04em] text-[#34373C]">
        Retention Rate
      </h3>

      <div class="relative">
        <button
          type="button"
          class="flex items-center gap-2 rounded-[3px] bg-[#5E5D5D] px-[12px] py-1 text-[10px] leading-none font-semibold text-white"
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

    <div class="relative mt-5 flex justify-center">
      <div class="relative h-[112px] w-[188px] overflow-hidden">
        <svg
          class="absolute inset-0"
          viewBox="0 0 200 118"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
          aria-hidden="true"
        >
          <defs>
            <linearGradient id="retentionArc" x1="31" y1="93" x2="105" y2="21" gradientUnits="userSpaceOnUse">
              <stop stop-color="#021717" />
              <stop offset="1" stop-color="#0B2A2A" />
            </linearGradient>
          </defs>

          <path
            d="M31 93 A69 69 0 0 1 169 93"
            pathLength="100"
            stroke="#E7EBF4"
            stroke-width="38"
            stroke-linecap="butt"
          />
          <path
            d="M31 93 A69 69 0 0 1 169 93"
            pathLength="100"
            :stroke-dasharray="`${currentValue} 100`"
            class="retention-fill"
            stroke="url(#retentionArc)"
            stroke-width="38"
            stroke-linecap="butt"
          />
        </svg>

        <span class="retention-indicator absolute h-[4px] w-[48px] rounded-full bg-[#132727]" :style="indicatorStyle" />

        <span class="absolute left-1/2 top-[58px] -translate-x-1/2 text-[27px] leading-none font-semibold text-[#34373C]">
          {{ currentValue }}
        </span>
      </div>
    </div>
  </article>
</template>

<style scoped>
.retention-fill {
  transition: stroke-dasharray 320ms ease;
}

.retention-indicator {
  transform-origin: center;
  transition: transform 320ms ease;
}
</style>
