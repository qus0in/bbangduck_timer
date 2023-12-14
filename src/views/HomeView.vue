<template>
  <div style="width: 100%">
    <div
      class="watch d-flex flex-column justify-content-center align-items-center pt-2"
      style="font-size: 25vw; aspect-ratio: 16/10"
    >
      <span @click="() => (isStarted ? stopTimer() : startTimer())">
        {{ displayHour }}:{{ displayMinute }}
      </span>
      <CContainer class="d-flex flex-column mt-3" style="height: 64px">
        <Transition>
          <CButtonGroup v-if="!isStarted" role="group" class="mt-2">
            <CButton
              @click="settingTime(minute)"
              color="light"
              style="font-size: 1.5rem"
              v-for="minute in minuteOptions"
              :key="minute"
            >
              {{ minute }} MIN
            </CButton>
          </CButtonGroup>
        </Transition>
      </CContainer>
    </div>
  </div>
</template>

<script setup lang="ts">
import { CButton, CButtonGroup, CContainer } from '@coreui/vue'
import { computed, ref } from 'vue'

const minuteOptions = [20, 15, 10]
const time = ref(60 * 20)
const digits = (t: number) => t.toLocaleString('en-US', { minimumIntegerDigits: 2 })
const displayHour = computed(() => digits(Math.floor(time.value / 60)))
const displayMinute = computed(() => digits(time.value % 60))
const settingTime = (v: number) => (time.value = v * 60)
const isStarted = ref(false)
let interval: any
const startTimer = () => {
  if (time.value === 0) return
  isStarted.value = true
  interval = setInterval(() => {
    if (time.value > 0) {
      time.value--
    } else {
      const audio = new Audio('./public/reception-bell.mp3')
      audio.play()
      isStarted.value = false
      clearInterval(interval)
    }
  }, 1000)
}
const stopTimer = () => {
  clearInterval(interval)
  isStarted.value = false
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&display=swap');

.watch {
  background-color: black;
  color: red;
  font-family: 'Share Tech Mono', monospace;
}

.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
</style>
