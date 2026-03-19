<script setup lang="ts">
import AppHeader from './AppHeader.vue'
import RetentionRateCard from './RetentionRateCard.vue'
import RevenueCard from './RevenueCard.vue'
import TopCustomersCard from './TopCustomersCard.vue'
import WeeklyTasksCard from './WeeklyTasksCard.vue'

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
  <main class="px-4 sm:px-8  xl:px-[52px] py-12">
    <AppHeader />

    <section class="grid gap-4 lg:grid-cols-3">
      <article
        class="rounded-[20px] bg-white pl-[38px] pr-[26.62px] pt-5 pb-6 shadow-[0_1px_7px_rgba(12,12,13,0.1)]"
      >
        <div class="flex items-center justify-between">
          <h3 class="text-sm font-medium text-[#34373C]">Total Leads</h3>
          <span class="rounded-[3px] bg-[#5E5D5D] px-2 py-[2px] text-[9px] font-medium text-white"
            >View</span
          >
        </div>
        <p class="mt-7 text-[50px] leading-[0.86] font-medium text-[#34373C]">
          4,100 <span class="text-[15px] font-semibold">Leads</span>
        </p>

        <div class="image-wrapper mt-4">
          <img class="w-full" src="@/assets/svg/barchart.svg" alt="bar chart" />
        </div>
      </article>

      <article
        class="lead-pipeline rounded-[20px] bg-[#F9FFFF] p-5 shadow-[0_1px_7px_rgba(12,12,13,0.1)]"
      >
        <div class="flex items-center justify-between">
          <h3 class="text-sm font-medium text-[#34373C]">Leads Pipeline</h3>
          <span class="rounded-[3px] bg-[#5E5D5D] px-2 py-[2px] text-[9px] font-medium text-white"
            >View</span
          >
        </div>

        <div class="mt-7 flex items-end -gap-2">
          <div v-for="stage in pipelineStages" :key="stage.label" class="flex flex-1 flex-col">
            <div class="flex justify-start">
              <span
                class="rounded-[4px] px-[8px] py-[3px] text-[10px] leading-none font-semibold text-white"
                :style="{ backgroundColor: stage.accent }"
              >
                {{ stage.value }}%
              </span>
            </div>
            <div class="relative mt-2 h-[58px]">
              <span
                class="absolute bottom-0 left-[6px] w-px bg-[#CFCFD1]"
                :style="{ height: `${stage.stemHeight}px` }"
              />
              <span class="absolute left-[2px] top-0 size-[9px] rounded-full bg-[#A0A2A6]" />
            </div>
          </div>
        </div>

        <div class="mt-[-1px] flex h-[16px] -gap-2 rounded-full bg-[#F5F5F5] p-[3px]">
          <div v-for="stage in pipelineStages" :key="`${stage.label}-bar`" class="flex-1">
            <span
              class="block h-full rounded-full transition-[width] duration-500 ease-out"
              :style="{
                width: `${stage.value}%`,
                backgroundColor: stage.accent,
              }"
            />
          </div>
        </div>

        <div class="mt-2 flex -gap-2">
          <small
            v-for="stage in pipelineStages"
            :key="`${stage.label}-label`"
            class="flex-1 text-[10px] text-[#616263]"
          >
            {{ stage.label }}
          </small>
        </div>
      </article>

      <article
        class="total-sales rounded-[20px] bg-[#F9FFFF] px-[17px] pt-[23px] pb-4 shadow-[0_1px_7px_rgba(12,12,13,0.1)]"
      >
       <div class="pl-5">
         <h3 class="text-[14px] font-medium tracking-[-0.02em] text-[#34373C]">Total Sales</h3>
         <p class="mt-8 flex items-end gap-[6px] text-[#34373C]">
           <span class="text-[46px] leading-[0.9] font-medium tracking-[-0.05em]">2,100K</span>
           <span class=" text-[15px] leading-none font-semibold">$</span>
         </p>
       </div>

        <button
          type="button"
          class="mt-5 flex h-[47px] w-full items-center justify-between rounded-full hover:bg-white/50 hover:ring hover:text-black bg-[#031D1D] py-[14px] pl-[18px] pr-[20.34px] text-left text-white"
        >
          <span class="text-[14px] font-normal tracking-[-0.02em]">View Chart</span>
          <span class="text-[22px] leading-none">
            <svg
              width="15"
              height="11"
              viewBox="0 0 15 11"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M8.01555 6.48031L10.2937 10.0531L14.1802 0.505421L3.88357 5.49067e-06L6.16167 3.57282L2.75541e-05 7.50162L1.85391 10.4091L8.01555 6.48031Z"
                fill="currentCOlor"
              />
            </svg>
          </span>
        </button>
      </article>
    </section>

    <section class="mt-4 grid gap-4 lg:grid-cols-3">
      <RevenueCard />

      <RetentionRateCard />

      <TopCustomersCard class="lg:row-span-2" />

      <WeeklyTasksCard />
    </section>
  </main>
</template>
