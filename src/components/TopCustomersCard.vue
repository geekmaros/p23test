<script setup lang="ts">
import { computed, onBeforeUnmount, onMounted, ref } from 'vue'
import avatarImage from '@/assets/images/user1.png'
import userImage from '@/assets/images/user2.png'

type CustomerRange = 'Weekly' | 'Daily' | 'Monthly'

type CustomerMetric = {
  label: string
  color: string
  value: number
  radius: number
}

type Customer = {
  id: number
  name: string
  category: string
  avatarType: 'image' | 'initials'
  avatarSrc?: string
  initials?: string
  avatarTone: string
  active?: boolean
}

const rangeOptions: CustomerRange[] = ['Weekly', 'Daily', 'Monthly']

const customerMetrics: Record<CustomerRange, CustomerMetric[]> = {
  Weekly: [
    { label: 'Customer 1', color: '#021717', value: 92, radius: 74 },
    { label: 'Customer 2', color: '#146AFA', value: 78, radius: 54 },
    { label: 'Customer 3', color: '#FD46E8', value: 64, radius: 34 },
  ],
  Daily: [
    { label: 'Customer 1', color: '#021717', value: 74, radius: 74 },
    { label: 'Customer 2', color: '#146AFA', value: 62, radius: 54 },
    { label: 'Customer 3', color: '#FD46E8', value: 49, radius: 34 },
  ],
  Monthly: [
    { label: 'Customer 1', color: '#021717', value: 99, radius: 74 },
    { label: 'Customer 2', color: '#146AFA', value: 83, radius: 54 },
    { label: 'Customer 3', color: '#FD46E8', value: 69, radius: 34 },
  ],
}

const customerLists: Record<CustomerRange, Customer[]> = {
  Weekly: [
    {
      id: 1,
      name: 'Lane Wade',
      category: 'E-commerce',
      avatarType: 'image',
      avatarSrc: avatarImage,
      avatarTone: 'from-[#E8C9FF] via-[#B38CEB] to-[#8C5FD3]',
    },
    {
      id: 2,
      name: 'Mina Cole',
      category: 'Retail',
      avatarType: 'image',
      avatarSrc: userImage,
      avatarTone: 'from-[#FFC885] via-[#FF8B2C] to-[#F05A1D]',
      active: true,
    },
    {
      id: 3,
      name: 'Tari Mensah',
      category: 'SaaS',
      avatarType: 'initials',
      initials: 'TM',
      avatarTone: 'from-[#C9FFF5] via-[#68C5BD] to-[#245E63]',
    },
  ],
  Daily: [
    {
      id: 1,
      name: 'Lane Wade',
      category: 'E-commerce',
      avatarType: 'image',
      avatarSrc: avatarImage,
      avatarTone: 'from-[#E8C9FF] via-[#B38CEB] to-[#8C5FD3]',
      active: true,
    },
    {
      id: 2,
      name: 'Tari Mensah',
      category: 'SaaS',
      avatarType: 'initials',
      initials: 'TM',
      avatarTone: 'from-[#C9FFF5] via-[#68C5BD] to-[#245E63]',
    },
    {
      id: 3,
      name: 'Mina Cole',
      category: 'Retail',
      avatarType: 'image',
      avatarSrc: userImage,
      avatarTone: 'from-[#FFC885] via-[#FF8B2C] to-[#F05A1D]',
    },
  ],
  Monthly: [
    {
      id: 1,
      name: 'Lane Wade',
      category: 'E-commerce',
      avatarType: 'image',
      avatarSrc: avatarImage,
      avatarTone: 'from-[#E8C9FF] via-[#B38CEB] to-[#8C5FD3]',
    },
    {
      id: 2,
      name: 'Mina Cole',
      category: 'Retail',
      avatarType: 'image',
      avatarSrc: userImage,
      avatarTone: 'from-[#FFC885] via-[#FF8B2C] to-[#F05A1D]',
      active: true,
    },
    {
      id: 3,
      name: 'Tari Mensah',
      category: 'SaaS',
      avatarType: 'initials',
      initials: 'TM',
      avatarTone: 'from-[#C9FFF5] via-[#68C5BD] to-[#245E63]',
    },
  ],
}

const selectedRange = ref<CustomerRange>('Weekly')
const menuOpen = ref(false)
const cardRef = ref<HTMLElement | null>(null)

const metrics = computed(() => customerMetrics[selectedRange.value])
const customers = computed(() => customerLists[selectedRange.value])
const totalScore = computed(() => Math.max(...metrics.value.map(({ value }) => value)))

function arcLength(radius: number) {
  return Math.PI * radius
}

function dashArray(radius: number, value: number) {
  const length = arcLength(radius)
  return `${(value / 100) * length} ${length}`
}

function selectRange(range: CustomerRange) {
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
        Top Customers
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

    <div class="mt-3 flex justify-center">
      <div class="relative h-[198px] w-[198px]">
        <svg
          class="absolute inset-0"
          viewBox="0 0 198 198"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
          aria-hidden="true"
        >
          <g transform="translate(99,99) rotate(-180)">
            <template v-for="metric in metrics" :key="metric.label">
              <circle
                cx="0"
                cy="0"
                :r="metric.radius"
                stroke="#F5F5F5"
                stroke-width="16"
                fill="none"
                stroke-linecap="round"
              />
              <circle
                cx="0"
                cy="0"
                :r="metric.radius"
                :stroke="metric.color"
                class="customer-ring"
                stroke-width="16"
                fill="none"
                stroke-linecap="round"
                :stroke-dasharray="dashArray(metric.radius, metric.value)"
              />
            </template>
          </g>
        </svg>

        <div class="absolute inset-[73px] rounded-full bg-white" />
        <span class="absolute left-1/2 top-1/2 -translate-x-1/2 -translate-y-1/2 text-[18px]  font-semibold tracking-[-0.04em] text-[#171717]">
          {{ totalScore }}%
        </span>
      </div>
    </div>

    <div class="mt-2 flex items-center justify-center gap-3 text-[10px] text-[#505258]">
      <span
        v-for="metric in metrics"
        :key="`${metric.label}-legend`"
        class="flex items-center gap-2"
      >
        <span class="size-3 rounded-full" :style="{ backgroundColor: metric.color }" />
        {{ metric.label }}
      </span>
    </div>

    <div class="mt-5 space-y-2">
      <div
        v-for="customer in customers"
        :key="customer.id"
        class="flex items-center justify-between rounded-[18px] px-2 py-2 transition"
        :class="customer.active ? 'bg-[#ECECEC]' : 'bg-transparent'"
      >
        <div class="flex items-center gap-3">
          <span
            class="grid size-[40px] place-items-center rounded-full bg-gradient-to-br p-[2px]"
            :class="customer.avatarTone"
          >
            <span class="grid size-full place-items-center rounded-full bg-white">
              <img
                v-if="customer.avatarType === 'image' && customer.avatarSrc"
                :src="customer.avatarSrc"
                :alt="customer.name"
                class="size-[32px] rounded-full object-cover"
              />
              <span
                v-else
                class="grid size-[32px] place-items-center rounded-full bg-[#1F6268] text-[11px] font-semibold text-white"
              >
                {{ customer.initials }}
              </span>
            </span>
          </span>

          <div>
            <p class="text-[15px] leading-4 font-semibold tracking-[-0.03em] text-[#34373C]">
              {{ customer.name }}
            </p>
            <small class="text-[10px] leading-none text-[#6A6C71]">{{ customer.category }}</small>
          </div>
        </div>

        <button
          type="button"
          class="grid size-7 place-items-center rounded-full text-[#55575C] transition hover:bg-white/80"
          :aria-label="`More options for ${customer.name}`"
        >
          <span class="flex items-center gap-[3px]">
            <span class="size-[3px] rounded-full bg-current" />
            <span class="size-[3px] rounded-full bg-current" />
            <span class="size-[3px] rounded-full bg-current" />
          </span>
        </button>
      </div>
    </div>
  </article>
</template>

<style scoped>
.customer-ring {
  transition: stroke-dasharray 320ms ease;
}
</style>
