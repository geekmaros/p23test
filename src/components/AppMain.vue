<script setup lang="ts">
import AppHeader from './AppHeader.vue'

const revenueDays = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
const revenueBars = [48, 90, 36, 100, 48, 90, 36]
const pipelineStages = [
  {
    label: 'Contacted',
    value: 70,
    accent: '#021717',
    stemHeight: 58,
  },
  {
    label: 'Qualify',
    value: 57,
    accent: '#FD46E8',
    stemHeight: 58,
  },
]
</script>

<template>
  <main class="px-4 py-6 sm:px-8 sm:py-10 xl:px-[52px] xl:py-12">
    <AppHeader />

    <section class="grid gap-4 lg:grid-cols-3">
      <article class="rounded-[20px] bg-white pl-[38px] pr-[26.62px] pt-5 pb-6 shadow-[0_1px_7px_rgba(12,12,13,0.1)]">
        <div class="flex items-center justify-between">
          <h3 class="text-sm font-medium text-[#34373C]">Total Leads</h3>
          <span class="rounded-[3px] bg-[#5E5D5D] px-2 py-[2px] text-[9px] font-medium text-white">View</span>
        </div>
        <p class="mt-7 text-[50px] leading-[0.86] font-medium text-[#34373C]">
          4,100 <span class="text-[15px] font-semibold">Leads</span>
        </p>

        <div class="image-wrapper mt-4">
          <img class="w-full" src="@/assets/svg/barchart.svg" alt="bar chart">
        </div>

      </article>

      <article class="lead-pipeline rounded-[20px] bg-white p-5 shadow-[0_1px_7px_rgba(12,12,13,0.1)]">
        <div class="flex items-center justify-between">
          <h3 class="text-sm font-medium text-[#34373C]">Leads Pipeline</h3>
          <span class="rounded-[3px] bg-[#5E5D5D] px-2 py-[2px] text-[9px] font-medium text-white">View</span>
        </div>

        <div class="mt-7 flex items-end gap-2">
          <div
            v-for="stage in pipelineStages"
            :key="stage.label"
            class="flex flex-1 flex-col"
          >
            <div class="flex justify-start">
              <span
                class="rounded-[4px] px-[8px] py-[3px] text-[10px] leading-none font-semibold text-white"
                :style="{ backgroundColor: stage.accent }"
              >
                {{ stage.value }}%
              </span>
            </div>
            <div class="relative mt-2 h-[74px]">
              <span
                class="absolute left-[6px] top-0 w-px bg-[#CFCFD1]"
                :style="{ height: `${stage.stemHeight}px` }"
              />
              <span class="absolute left-[2px] top-[0] size-[9px] rounded-full bg-[#A0A2A6]" />
              <span
                class="absolute bottom-0 left-0 h-[12px] rounded-full transition-[width] duration-500 ease-out"
                :style="{
                  width: `${stage.value}%`,
                  backgroundColor: stage.accent,
                }"
              />
            </div>
            <small class="mt-2 text-[10px] text-[#616263]">{{ stage.label }}</small>
          </div>
        </div>
      </article>

      <article class="rounded-[20px] bg-white p-5 shadow-[0_1px_7px_rgba(12,12,13,0.1)]">
        <div class="flex items-center justify-between">
          <h3 class="text-sm font-medium text-[#34373C]">Revenue</h3>
          <span class="rounded-[3px] bg-[#5E5D5D] px-2 py-[2px] text-[9px] font-medium text-white">Weekly</span>
        </div>
        <div class="mt-4 flex gap-3">
          <div class="flex h-[156px] flex-col justify-between text-[10px] text-[#616263]">
            <small>$340</small>
            <small>300$</small>
            <small>200$</small>
            <small>100$</small>
          </div>
          <div class="flex h-[156px] flex-1 items-end gap-[7px]">
            <div v-for="(bar, i) in revenueBars" :key="revenueDays[i]" class="flex flex-col items-center gap-2">
              <span
                :style="{ height: `${bar}px` }"
                :class="[
                  'w-[29px] rounded-[10px]',
                  revenueDays[i] === 'Thu' ? 'bg-[#021717]' : 'bg-[#F5F5F5]',
                ]"
              />
              <small class="text-[9px] font-medium text-[#34373C]">{{ revenueDays[i] }}</small>
            </div>
          </div>
        </div>
      </article>
    </section>

    <section class="mt-4 grid gap-4 lg:grid-cols-3">
      <article class="rounded-[20px] bg-[#021717] p-5 text-white shadow-[0_1px_7px_rgba(12,12,13,0.1)]">
        <h3 class="text-sm font-medium">Weekly Tasks</h3>
        <div class="mt-5 flex gap-8">
          <div>
            <p class="text-[50px] leading-[0.86] font-medium">70%</p>
            <small class="text-[10px]">Task Completed</small>
          </div>
          <div>
            <p class="text-[50px] leading-[0.86] font-medium">31%</p>
            <small class="text-[10px]">Better than previous month</small>
          </div>
        </div>
        <div class="mt-6 w-fit rounded-full bg-[#F5F5F5] px-3 py-[7px] text-[10px] font-medium text-[#021717]">
          Your work balance this week. Awesome!
        </div>
      </article>

      <article class="rounded-[20px] bg-white p-5 shadow-[0_1px_7px_rgba(12,12,13,0.1)]">
        <div class="flex items-center justify-between">
          <h3 class="text-sm font-medium text-[#34373C]">Retention Rate</h3>
          <span class="rounded-[3px] bg-[#5E5D5D] px-2 py-[2px] text-[9px] font-medium text-white">Weekly</span>
        </div>
        <div class="mt-5 grid place-items-center">
          <div class="relative grid size-[156px] place-items-center rounded-full bg-[conic-gradient(#021717_260deg,#E8EBEC_0deg)]">
            <div class="size-[114px] rounded-full bg-white" />
            <span class="absolute text-4xl font-bold text-[#34373C]">58</span>
          </div>
        </div>
      </article>

      <article class="rounded-[20px] bg-white p-5 shadow-[0_1px_7px_rgba(12,12,13,0.1)]">
        <div class="mb-4 flex items-center justify-between">
          <h3 class="text-sm font-medium text-[#34373C]">Top Customers</h3>
          <span class="rounded-[3px] bg-[#5E5D5D] px-2 py-[2px] text-[9px] font-medium text-white">Weekly</span>
        </div>
        <div class="space-y-2">
          <div class="flex items-center gap-3 rounded-xl bg-[#ECECEC] p-2">
            <span class="size-[34px] rounded-full bg-gradient-to-br from-[#f5f5f5] to-[#4f7f81]" />
            <div>
              <p class="text-[15px] leading-4 font-semibold text-[#34373C]">Lane Wade</p>
              <small class="text-[10px] text-[#34373C]">E-commerce</small>
            </div>
          </div>

          <div class="flex items-center gap-3 rounded-xl p-2">
            <span class="size-[34px] rounded-full bg-gradient-to-br from-[#f5f5f5] to-[#4f7f81]" />
            <div>
              <p class="text-[15px] leading-4 font-semibold text-[#34373C]">Lane Wade</p>
              <small class="text-[10px] text-[#34373C]">E-commerce</small>
            </div>
          </div>
        </div>
      </article>
    </section>
  </main>
</template>
