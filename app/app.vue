<template>
  <section class="min-h-screen flex items-center justify-center bg-neutral-950">
    <div class="w-full h-full">
      <div class="w-full md:px-16 px-10 md:pt-16 pt-10 pb-10 flex-col justify-end items-center lg:gap-28 md:gap-16 gap-10 inline-flex">
        <div class="flex-col justify-end items-center lg:gap-16 gap-10 flex">
          <div class="flex-col justify-center items-center gap-10 flex">
            <div class="flex-col justify-start items-center gap-2.5 flex">
              <h2 class="text-center text-neutral-50 md:text-6xl text-5xl font-bold font-manrope leading-normal">Coming Soon</h2>
              <p class="text-center text-gray-500 text-base font-normal leading-relaxed">Just {{ days }} days remaining until the big reveal of our my new website!</p>
            </div>
            <div class="flex items-start justify-center w-full gap-2 count-down-main">
              <div class="timer flex flex-col gap-0.5">
                <div class="">
                  <h3 class="text-center text-white text-2xl font-bold font-manrope leading-9">{{ formattedDays }}</h3>
                </div>
                <p class="text-center text-gray-500 text-xs font-normal leading-normal w-full">DAYS</p>
              </div>
              <h3 class="w-3 text-center text-gray-500 text-2xl font-medium font-manrope leading-9">:</h3>
              <div class="timer flex flex-col gap-0.5">
                <div class="">
                  <h3 class="text-center text-white text-2xl font-bold font-manrope leading-9">{{ formattedHours }}</h3>
                </div>
                <p class="text-center text-gray-500 text-xs font-normal leading-normal w-full">HRS</p>
              </div>
              <h3 class="w-3 text-center text-gray-500 text-2xl font-medium font-manrope leading-9">:</h3>
              <div class="timer flex flex-col gap-0.5">
                <div class="">
                  <h3 class="text-center text-white text-2xl font-bold font-manrope leading-9">{{ formattedMinutes }}</h3>
                </div>
                <p class="text-center text-gray-500 text-xs font-normal leading-normal w-full">MINS</p>
              </div>
              <h3 class="w-3 text-center text-gray-500 text-2xl font-medium font-manrope leading-9">:</h3>
              <div class="timer flex flex-col gap-0.5">
                <div class="">
                  <h3 class="text-center text-white text-2xl font-bold font-manrope leading-9">{{ formattedSeconds }}</h3>
                </div>
                <p class="text-center text-gray-500 text-xs font-normal leading-normal w-full">SECS</p>
              </div>
            </div>
            <div class="w-full flex-col justify-center items-center gap-5 flex">
              <h6 class="text-center text-neutral-50 text-base font-semibold leading-relaxed">Launched Date: August 28, 2025</h6>
              <div class="justify-center items-center gap-2.5 flex sm:flex-row flex-col">
                <input
                    v-model="email"
                    type="email"
                    class="w-80 focus:outline-none px-3.5 py-2 shadow-[0px_1px_2px_0px_rgba(16,_24,_40,_0.05)] text-gray-900 placeholder-gray-400 text-sm font-normal leading-relaxed h-10 bg-white rounded-lg border border-gray-200 justify-start items-center gap-1.5 inline-flex"
                    placeholder="Type your mail..."
                >
                <button
                    @click="handleNotifyMe"
                    class="sm:w-fit w-full px-3.5 py-2 bg-neutral-50 hover:bg-neutral-400 transition-all duration-700 ease-in-out rounded-lg shadow-[0px_1px_2px_0px_rgba(16,_24,_40,_0.05)] justify-center items-center flex cursor-pointer"
                >
                  <span class="px-1.5 text-gray-900 text-sm font-medium leading-6 whitespace-nowrap">Notify Me</span>
                </button>
              </div>
            </div>
          </div>
        </div>
        <p class="text-center text-gray-500 text-sm font-normal leading-snug mt-auto">
          Get in touch with us:
          <a href="mailto:mail@pagedone.com" class="hover:text-gray-100 transition-all duration-700 ease-in-out">contact@aristidehervembassi.com</a>
        </p>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'

// Reactive state
const days = ref<number>(0)
const hours = ref<number>(0)
const minutes = ref<number>(0)
const seconds = ref<number>(0)
const email = ref<string>('')
const countdownInterval = ref<NodeJS.Timeout | null>(null)
const targetDate = ref<Date>(new Date('August 28, 2025 00:59:59'))

// Computed properties for formatted time values
const formattedDays = computed<string>(() =>
    days.value < 10 ? `0${days.value}` : days.value.toString()
)

const formattedHours = computed<string>(() =>
    hours.value < 10 ? `0${hours.value}` : hours.value.toString()
)

const formattedMinutes = computed<string>(() =>
    minutes.value < 10 ? `0${minutes.value}` : minutes.value.toString()
)

const formattedSeconds = computed<string>(() =>
    seconds.value < 10 ? `0${seconds.value}` : seconds.value.toString()
)

// Utility functions
const isValidEmail = (emailAddress: string): boolean => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  return emailRegex.test(emailAddress)
}

const resetToNextMonth = (): void => {
  const nextMonthDate = new Date()
  nextMonthDate.setMonth(nextMonthDate.getMonth() + 1)

  // If the current month is December, set the destination date to the same day next year
  if (nextMonthDate.getMonth() === 0) {
    nextMonthDate.setFullYear(nextMonthDate.getFullYear() + 1)
  }

  targetDate.value = nextMonthDate
}

const updateCountdown = (): void => {
  const now = new Date().getTime()
  const diff = targetDate.value.getTime() - now

  // Check if the countdown has reached zero or negative
  if (diff <= 0) {
    resetToNextMonth()
    return
  }

  days.value = Math.floor(diff / (1000 * 60 * 60 * 24))
  hours.value = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60))
  minutes.value = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60))
  seconds.value = Math.floor((diff % (1000 * 60)) / 1000)
}

const startCountdown = (): void => {
  updateCountdown() // Initial update

  countdownInterval.value = setInterval(() => {
    updateCountdown()
  }, 1000)
}

const handleNotifyMe = (): void => {
  if (!email.value) {
    alert('Please enter your email address')
    return
  }

  if (!isValidEmail(email.value)) {
    alert('Please enter a valid email address')
    return
  }

  // Here you would typically send the email to your backend
  console.log('Email submitted:', email.value)
  alert('Thank you! We\'ll notify you when we launch.')
  email.value = ''
}

// Lifecycle hooks
onMounted(() => {
  startCountdown()
})

onUnmounted(() => {
  if (countdownInterval.value) {
    clearInterval(countdownInterval.value)
  }
})
</script>

<style scoped>
/* Add any custom styles here if needed */
.font-manrope {
  font-family: 'Manrope', sans-serif;
}

</style>