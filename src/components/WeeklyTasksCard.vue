<script setup lang="ts">
import { computed, onBeforeUnmount, onMounted, ref } from 'vue'
import user1Image from '@/assets/images/user1.png'
import user2Image from '@/assets/images/user2.png'

type MeetingRange = 'Daily' | 'Weekly' | 'Monthly'

const rangeOptions: MeetingRange[] = ['Daily', 'Weekly', 'Monthly']

const meetingsByRange: Record<
  MeetingRange,
  Array<{
    id: number
    name: string
    category: string
    avatarSrc: string
  }>
> = {
  Daily: [
    {
      id: 1,
      name: 'Lane Wade',
      category: 'E-commerce',
      avatarSrc: user1Image,
    },
    {
      id: 2,
      name: 'Lane Wade',
      category: 'E-commerce',
      avatarSrc: user2Image,
    },
  ],
  Weekly: [
    {
      id: 1,
      name: 'Mina Cole',
      category: 'Retail',
      avatarSrc: user1Image,
    },
    {
      id: 2,
      name: 'Tari Mensah',
      category: 'SaaS',
      avatarSrc: user2Image,
    },
  ],
  Monthly: [
    {
      id: 1,
      name: 'Lane Wade',
      category: 'E-commerce',
      avatarSrc: user1Image,
    },
    {
      id: 2,
      name: 'Tari Mensah',
      category: 'SaaS',
      avatarSrc: user2Image,
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
          <span class="grid size-6 place-items-center rounded-full ">
            <svg
              width="24"
              height="24"
              viewBox="0 0 24 24"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M12 22C17.5228 22 22 17.5228 22 12C22 6.47715 17.5228 2 12 2C6.47715 2 2 6.47715 2 12C2 17.5228 6.47715 22 12 22Z"
                stroke="#021717"
                stroke-width="1.5"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
              <path
                d="M16 9V9.01"
                stroke="#021717"
                stroke-width="2.5"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
              <path
                d="M8 9V9.01"
                stroke="#021717"
                stroke-width="2.5"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
              <path
                d="M8 14.5C8.91212 15.7144 10.3643 16.5 12 16.5C13.6357 16.5 15.0879 15.7144 16 14.5"
                stroke="#021717"
                stroke-width="1.5"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
            </svg>
          </span>
          <span>Your work balance this week. Awesome!</span>
        </div>
      </section>

      <section class="rounded-[22px] h-full flex flex-col px-4 ">
        <div class="flex items-start justify-between">
          <div>
            <h4 class="text-[10px] leading-none text-white/95">Scheduled Meetings</h4>
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

        <div class="mt-3 space-y-3 py-2 pl-[9px] pr-[6px] rounded-[20px] h-full bg-[#123B3D]">
          <div
            v-for="meeting in scheduledMeetings"
            :key="meeting.id"
            class="flex items-center justify-between rounded-[18px] bg-[#F9FFFF] px-3 py-2"
          >
            <div class="flex items-center gap-3">
              <img
                :src="meeting.avatarSrc"
                :alt="meeting.name"
                class="size-[38px] rounded-full object-cover"
              />

              <div>
                <p class="text-[15px] leading-none font-semibold tracking-[-0.03em] text-[#1E2328]">
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
                <svg width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path d="M16.5 9C16.5 4.85786 13.1421 1.5 9 1.5C4.85786 1.5 1.5 4.85786 1.5 9C1.5 13.1421 4.85786 16.5 9 16.5C13.1421 16.5 16.5 13.1421 16.5 9Z" stroke="#F1CAC4" stroke-linecap="round" stroke-linejoin="round"/>
                  <path d="M11.2496 11.25L6.75 6.75M6.75048 11.25L11.25 6.75" stroke="#F1CAC4" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>

              </button>

              <button
                type="button"
                class="grid size-5 place-items-center"
                :aria-label="`Open video call for ${meeting.name}`"
              >
                <svg width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path d="M8.25 6H9.75" stroke="#84A9E5" stroke-linecap="round"/>
                  <path d="M1.5 8.25C1.5 5.77513 1.5 4.53769 2.26885 3.76885C3.03769 3 4.27513 3 6.75 3H7.5C9.97485 3 11.2123 3 11.9812 3.76885C12.75 4.53769 12.75 5.77513 12.75 8.25V9.75C12.75 12.2248 12.75 13.4623 11.9812 14.2312C11.2123 15 9.97485 15 7.5 15H6.75C4.27513 15 3.03769 15 2.26885 14.2312C1.5 13.4623 1.5 12.2248 1.5 9.75V8.25Z" stroke="#84A9E5"/>
                  <path d="M12.75 6.67939L12.8444 6.60147C14.4313 5.29217 15.2247 4.63752 15.8623 4.95362C16.5 5.26971 16.5 6.31766 16.5 8.41358V9.58643C16.5 11.6824 16.5 12.7303 15.8623 13.0464C15.2247 13.3625 14.4313 12.7079 12.8444 11.3985L12.75 11.3206" stroke="#84A9E5" stroke-linecap="round"/>
                </svg>

              </button>
            </div>
          </div>
        </div>
      </section>
    </div>
  </article>
</template>
