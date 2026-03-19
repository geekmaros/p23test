<script setup lang="ts">
import { computed, onBeforeUnmount, onMounted, ref } from 'vue'

type MeetingRange = 'Daily' | 'Weekly' | 'Monthly'

const rangeOptions: MeetingRange[] = ['Daily', 'Weekly', 'Monthly']

const meetingsByRange: Record<
  MeetingRange,
  Array<{
    id: number
    name: string
    category: string
    avatar: string
    avatarBg: string
  }>
> = {
  Daily: [
    {
      id: 1,
      name: 'Lane Wade',
      category: 'E-commerce',
      avatar: 'LW',
      avatarBg: 'from-[#6479A4] via-[#A7B4DB] to-[#F4D1C3]',
    },
    {
      id: 2,
      name: 'Lane Wade',
      category: 'E-commerce',
      avatar: 'LW',
      avatarBg: 'from-[#D78617] via-[#F0B348] to-[#F6D18A]',
    },
  ],
  Weekly: [
    {
      id: 1,
      name: 'Mina Cole',
      category: 'Retail',
      avatar: 'MC',
      avatarBg: 'from-[#C39D62] via-[#EDC987] to-[#F2DFB4]',
    },
    {
      id: 2,
      name: 'Tari Mensah',
      category: 'SaaS',
      avatar: 'TM',
      avatarBg: 'from-[#4C7C7E] via-[#8BBEC0] to-[#CFEDEE]',
    },
  ],
  Monthly: [
    {
      id: 1,
      name: 'Lane Wade',
      category: 'E-commerce',
      avatar: 'LW',
      avatarBg: 'from-[#6479A4] via-[#A7B4DB] to-[#F4D1C3]',
    },
    {
      id: 2,
      name: 'Tari Mensah',
      category: 'SaaS',
      avatar: 'TM',
      avatarBg: 'from-[#4C7C7E] via-[#8BBEC0] to-[#CFEDEE]',
    },
  ],
}

const selectedRange = ref<MeetingRange>('Daily')
const menuOpen = ref(false)
const cardRef = ref<HTMLElement | null>(null)

const scheduledMeetings = computed(() => meetingsByRange[selectedRange.value])

function selectRange(range: MeetingRange) {
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
    class="rounded-[20px] bg-[#021717] px-6 pt-5 pb-5 text-white shadow-[0_1px_7px_rgba(12,12,13,0.1)] lg:col-span-2"
  >
    <div class="grid gap-5 lg:grid-cols-[1fr_292px] lg:items-start">
      <section>
        <h3 class="text-[14px] leading-none font-medium tracking-[-0.03em]">Weekly Tasks</h3>

        <div class="mt-11 flex items-end gap-8">
          <div>
            <p class="text-[54px] leading-[0.88] font-medium tracking-[-0.05em]">70%</p>
            <small class="mt-1 block text-[11px] leading-none text-white/92">Task Completed</small>
          </div>

          <div>
            <p class="text-[54px] leading-[0.88] font-medium tracking-[-0.05em]">31%</p>
            <small class="mt-1 block text-[11px] leading-none text-white/92">
              Better than previous month
            </small>
          </div>
        </div>

        <div
          class="mt-8 inline-flex items-center gap-2 rounded-full bg-white px-4 py-[7px] text-[11px] font-medium text-[#1E2328]"
        >
          <span class="grid size-6 place-items-center rounded-full border border-[#1E2328]">
            <svg
              width="14"
              height="14"
              viewBox="0 0 14 14"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <circle cx="7" cy="7" r="6" stroke="currentColor" stroke-width="1.2" />
              <circle cx="4.8" cy="5.6" r="0.7" fill="currentColor" />
              <circle cx="9.2" cy="5.6" r="0.7" fill="currentColor" />
              <path
                d="M4.6 8.7C5.1 9.45 5.95 9.8 7 9.8C8.05 9.8 8.9 9.45 9.4 8.7"
                stroke="currentColor"
                stroke-width="1.2"
                stroke-linecap="round"
              />
            </svg>
          </span>
          <span>Your work balance this week. Awesome!</span>
        </div>
      </section>

      <section class="rounded-[22px] bg-[#123B3D] px-4 pt-4 pb-3">
        <div class="flex items-start justify-between">
          <div>
            <h4 class="text-[12px] leading-none font-medium text-white/95">Scheduled Meetings</h4>
          </div>

          <div class="relative">
            <button
              type="button"
              class="flex items-center gap-2 rounded-[3px] bg-[#5E5D5D] px-[10px] py-1 text-[10px] leading-none font-semibold text-white"
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

        <div class="mt-3 space-y-3">
          <div
            v-for="meeting in scheduledMeetings"
            :key="meeting.id"
            class="flex items-center justify-between rounded-[18px] bg-[#F9FFFF] px-3 py-2"
          >
            <div class="flex items-center gap-3">
              <span class="grid size-[38px] place-items-center rounded-full bg-gradient-to-br text-[12px] font-semibold text-[#1B2428]" :class="meeting.avatarBg">
                {{ meeting.avatar }}
              </span>

              <div>
                <p class="text-[14px] leading-none font-semibold tracking-[-0.03em] text-[#1E2328]">
                  {{ meeting.name }}
                </p>
                <small class="mt-1 block text-[10px] leading-none text-[#6C7075]">
                  {{ meeting.category }}
                </small>
              </div>
            </div>

            <div class="flex items-center gap-3 text-[#9CB7F7]">
              <button
                type="button"
                class="grid size-5 place-items-center rounded-full text-[#F0BBB1]"
                :aria-label="`Decline meeting with ${meeting.name}`"
              >
                <svg
                  width="14"
                  height="14"
                  viewBox="0 0 14 14"
                  fill="none"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <circle cx="7" cy="7" r="6" stroke="currentColor" stroke-width="1" />
                  <path d="M4.8 4.8L9.2 9.2M9.2 4.8L4.8 9.2" stroke="currentColor" stroke-width="1" stroke-linecap="round" />
                </svg>
              </button>

              <button
                type="button"
                class="grid size-5 place-items-center"
                :aria-label="`Open video call for ${meeting.name}`"
              >
                <svg
                  width="16"
                  height="12"
                  viewBox="0 0 16 12"
                  fill="none"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <rect x="1" y="2" width="10" height="8" rx="2" stroke="currentColor" stroke-width="1.1" />
                  <path d="M11 4.3L14.2 2.8V9.2L11 7.7V4.3Z" stroke="currentColor" stroke-width="1.1" stroke-linejoin="round" />
                </svg>
              </button>
            </div>
          </div>
        </div>
      </section>
    </div>
  </article>
</template>
