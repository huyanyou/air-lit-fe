<script setup lang="ts">
const props = defineProps<{ showCourseAction: boolean }>();
const { parsedCourseList, originalWeekIndex, currentWeekIndex } = storeToRefs(
  useCourseStore()
);
const { setCurrentWeekIndex } = useCourseStore();
const scrollTo = ref("week0");
watch(
  () => +props.showCourseAction + currentWeekIndex.value,
  () => {
    if (props.showCourseAction) scrollTo.value = `week${currentWeekIndex.value - 1}`;
  }
);
</script>

<template>
  <scroll-view
    class="transition-height duration-300 overflow-scroll whitespace-nowrap"
    :class="showCourseAction ? 'h-20' : 'h-0'"
    scroll-x
    scroll-with-animation
    :scroll-into-view="scrollTo"
  >
    <template v-for="(weeksTimetable, weeksIndex) of parsedCourseList" :key="weeksIndex">
      <div
        :id="`week${weeksIndex + 1}`"
        class="py-1 px-2 inline-block"
        @click="setCurrentWeekIndex(weeksIndex)"
      >
        <div
          class="rounded-lg py-1 px-2"
          :class="
            originalWeekIndex === weeksIndex
              ? 'bg-gray-400/50 dark:!bg-op60'
              : currentWeekIndex === weeksIndex
              ? 'bg-gray-300 bg-op80 dark:!bg-op20'
              : ''
          "
        >
          <div class="text-xs text-center mb-1">
            {{ `第${weeksIndex + 1}周` }}
          </div>
          <div class="h-10 w-10" grid="~ flow-col cols-5 rows-5">
            <template
              v-for="(weekWeekTimetable, weekWeekIndex) of weeksTimetable"
              :key="weekWeekIndex"
            >
              <template v-if="weekWeekIndex < 5">
                <template v-for="(item, _idx) of weekWeekTimetable" :key="_idx">
                  <div
                    class="rounded-full mx-auto h-1.5 w-1.5"
                    :class="item ? 'bg-light-blue-500' : 'bg-gray-200'"
                  />
                </template>
              </template>
            </template>
          </div>
        </div>
      </div>
    </template>
  </scroll-view>
</template>
