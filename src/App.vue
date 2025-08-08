<template>
  <div>
    <!-- Фоновый градиент -->
    <div class="bg-gradient"></div>

    <!-- Анимированные частицы -->
    <div class="particles">
      <div
        v-for="particle in particles"
        :key="particle.id"
        class="particle"
        :style="particle.style"
      ></div>
    </div>

    <div class="container">
      <!-- Заголовок -->
      <header class="header">
        <h1 class="logo"><i class="fas fa-gem"></i> MEGA LOTTERY</h1>
        <p class="tagline">Твой шанс изменить жизнь навсегда!</p>
      </header>

      <!-- Главный баннер -->
      <div class="banner">
        <h2 class="banner-text"><i class="fas fa-trophy"></i> ДЖЕКПОТ</h2>
        <div class="jackpot-amount">{{ formatCurrency(jackpot) }}</div>
        <p class="banner-text">До розыгрыша осталось:</p>

        <div class="countdown">
          <div class="countdown-item">
            <span class="countdown-number">{{ timeLeft.days }}</span>
            <span class="countdown-label">дней</span>
          </div>
          <div class="countdown-item">
            <span class="countdown-number">{{ timeLeft.hours }}</span>
            <span class="countdown-label">часов</span>
          </div>
          <div class="countdown-item">
            <span class="countdown-number">{{ timeLeft.minutes }}</span>
            <span class="countdown-label">минут</span>
          </div>
          <div class="countdown-item">
            <span class="countdown-number">{{ timeLeft.seconds }}</span>
            <span class="countdown-label">секунд</span>
          </div>
        </div>
      </div>

      <!-- Лотерейный билет -->
      <div class="lottery-ticket">
        <div class="ticket-header">
          <h3 class="ticket-title">
            <i class="fas fa-ticket-alt"></i> Лотерейный билет
          </h3>
          <p class="draw-date">Розыгрыш: {{ drawDate }}</p>
        </div>

        <div class="ticket-body">
          <div class="numbers">
            <div
              v-for="(number, index) in numbers"
              :key="index"
              class="number-ball"
              :style="{ animationDelay: `${index * 0.1}s` }"
            >
              {{ number }}
            </div>
          </div>

          <div class="barcode">
            <div
              v-for="i in 20"
              :key="i"
              class="bar"
              :style="{ height: `${20 + Math.random() * 40}px` }"
            ></div>
          </div>
        </div>

        <div class="ticket-footer">
          <button class="action-btn" @click="validateTicket">
            <i class="fas fa-check-circle"></i> Проверить билет
          </button>
        </div>
      </div>

      <!-- Особенности -->
      <div class="features">
        <div class="feature-card">
          <div class="feature-icon">
            <i class="fas fa-bolt"></i>
          </div>
          <h4 class="feature-title">Мгновенные результаты</h4>
          <p class="feature-text">
            Узнайте результат сразу после розыгрыша. Никакого ожидания!
          </p>
        </div>

        <div class="feature-card">
          <div class="feature-icon">
            <i class="fas fa-shield-alt"></i>
          </div>
          <h4 class="feature-title">100% Безопасность</h4>
          <p class="feature-text">
            Все транзакции защищены современным шифрованием.
          </p>
        </div>

        <div class="feature-card">
          <div class="feature-icon">
            <i class="fas fa-gift"></i>
          </div>
          <h4 class="feature-title">Множество призов</h4>
          <p class="feature-text">
            Более 1000 призов в каждом розыгрыше. Каждый может выиграть!
          </p>
        </div>
      </div>
    </div>
  </div>
  <Footer />
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from "vue";
import Footer from "./components/Footer.vue";
// Интерфейсы
interface Particle {
  id: number;
  style: {
    left: string;
    top: string;
    width: string;
    height: string;
    animationDuration: string;
    animationDelay: string;
  };
}

interface TimeLeft {
  days: number;
  hours: number;
  minutes: number;
  seconds: number;
}

// Реактивные данные
const drawDate = ref<string>("");
const numbers = ref<number[]>([]);
const jackpot = ref<number>(125000000);
const particles = ref<Particle[]>([]);
const timeLeft = ref<TimeLeft>({
  days: 3,
  hours: 14,
  minutes: 27,
  seconds: 45,
});

// Переменная для таймера
let countdownTimer: NodeJS.Timeout | null = null;

// Функция генерации чисел лотереи
function generateNumbers(): number[] {
  const nums: number[] = [];
  while (nums.length < 6) {
    const n = Math.floor(Math.random() * 49) + 1;
    if (!nums.includes(n)) nums.push(n);
  }
  return nums.sort((a, b) => a - b);
}

// Функция форматирования валюты
function formatCurrency(amount: number): string {
  return new Intl.NumberFormat("ru-RU", {
    style: "currency",
    currency: "RUB",
    minimumFractionDigits: 0,
    maximumFractionDigits: 0,
  }).format(amount);
}

// Функция проверки билета
function validateTicket(): void {
  alert("Билет проверен! Удачи в розыгрыше! 🍀");
  numbers.value = generateNumbers();
}

// Функция создания анимированных частиц
function createParticles(): void {
  particles.value = [];
  for (let i = 0; i < 20; i++) {
    particles.value.push({
      id: i,
      style: {
        left: `${Math.random() * 100}%`,
        top: `${Math.random() * 100}%`,
        width: `${2 + Math.random() * 4}px`,
        height: `${2 + Math.random() * 4}px`,
        animationDuration: `${4 + Math.random() * 4}s`,
        animationDelay: `${Math.random() * 2}s`,
      },
    });
  }
}

// Функция запуска таймера обратного отсчета
function startCountdown(): void {
  countdownTimer = setInterval(() => {
    if (timeLeft.value.seconds > 0) {
      timeLeft.value.seconds--;
    } else if (timeLeft.value.minutes > 0) {
      timeLeft.value.minutes--;
      timeLeft.value.seconds = 59;
    } else if (timeLeft.value.hours > 0) {
      timeLeft.value.hours--;
      timeLeft.value.minutes = 59;
      timeLeft.value.seconds = 59;
    } else if (timeLeft.value.days > 0) {
      timeLeft.value.days--;
      timeLeft.value.hours = 23;
      timeLeft.value.minutes = 59;
      timeLeft.value.seconds = 59;
    }
  }, 1000);
}

// Хук монтирования
onMounted(() => {
  numbers.value = generateNumbers();
  createParticles();
  startCountdown();

  // Установка даты розыгрыша
  const nextDrawDate = new Date();
  nextDrawDate.setDate(nextDrawDate.getDate() + 3);
  drawDate.value = nextDrawDate.toLocaleDateString("ru-RU");
});

// Хук размонтирования
onUnmounted(() => {
  if (countdownTimer) {
    clearInterval(countdownTimer);
  }
});
</script>

<style lang="scss" scoped>
// Основные стили
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.bg-gradient {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #0f0f0f 0%, #1a0a2e 50%, #16213e 100%);
  z-index: -1;
}

.particles {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 1;
}

.particle {
  position: absolute;
  background: radial-gradient(circle, #ffd700, #ff6b6b);
  border-radius: 50%;
  opacity: 0.8;
  animation: float 6s ease-in-out infinite;
}

@keyframes float {
  0%,
  100% {
    transform: translateY(0px) rotate(0deg);
    opacity: 0.8;
  }
  50% {
    transform: translateY(-20px) rotate(180deg);
    opacity: 0.4;
  }
}

.container {
  position: relative;
  z-index: 2;
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  min-height: 100vh;
}

.header {
  text-align: center;
  padding: 40px 0;
}

.logo {
  font-size: 3rem;
  font-weight: bold;
  background: linear-gradient(45deg, #ffd700, #ff6b6b, #4ecdc4);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  margin-bottom: 10px;
  text-shadow: 0 0 30px rgba(255, 215, 0, 0.5);
}

.tagline {
  color: #ffffff;
  font-size: 1.2rem;
  opacity: 0.9;
}

.banner {
  background: linear-gradient(
    135deg,
    rgba(255, 215, 0, 0.1) 0%,
    rgba(255, 107, 107, 0.1) 100%
  );
  backdrop-filter: blur(20px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 20px;
  padding: 40px;
  margin: 40px 0;
  text-align: center;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
  position: relative;
  overflow: hidden;

  &::before {
    content: "";
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(
      90deg,
      transparent,
      rgba(255, 255, 255, 0.1),
      transparent
    );
    animation: shine 3s infinite;
  }
}

@keyframes shine {
  0% {
    left: -100%;
  }
  100% {
    left: 100%;
  }
}

.jackpot-amount {
  font-size: 4rem;
  font-weight: bold;
  color: #ffd700;
  text-shadow: 0 0 20px rgba(255, 215, 0, 0.8);
  margin: 20px 0;
}

.banner-text {
  color: #ffffff;
  font-size: 1.5rem;
  margin: 20px 0;
}

.countdown {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin: 30px 0;
}

.countdown-item {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  padding: 20px;
  text-align: center;
  min-width: 80px;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.countdown-number {
  font-size: 2rem;
  font-weight: bold;
  color: #ffd700;
  display: block;
}

.countdown-label {
  color: #ffffff;
  font-size: 0.9rem;
  opacity: 0.8;
}

.lottery-ticket {
  max-width: 400px;
  margin: 40px auto;
  background: linear-gradient(135deg, #1a1a1a 0%, #2d2d2d 100%);
  border-radius: 20px;
  overflow: hidden;
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.5);
  border: 2px solid rgba(255, 215, 0, 0.3);
  position: relative;

  &::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: linear-gradient(
      45deg,
      transparent,
      rgba(255, 215, 0, 0.1),
      transparent
    );
    pointer-events: none;
  }
}

.ticket-header {
  background: linear-gradient(135deg, #ff6b6b, #ee5a52);
  padding: 20px;
  text-align: center;
  position: relative;
}

.ticket-title {
  color: #ffffff;
  font-size: 1.8rem;
  font-weight: bold;
  margin-bottom: 10px;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
}

.draw-date {
  color: rgba(255, 255, 255, 0.9);
  font-size: 1rem;
}

.ticket-body {
  padding: 30px;
  background: linear-gradient(135deg, #2d2d2d 0%, #1a1a1a 100%);
}

.numbers {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  gap: 15px;
  margin-bottom: 30px;
}

.number-ball {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background: linear-gradient(135deg, #ffd700, #ffed4e);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.2rem;
  font-weight: bold;
  color: #333;
  box-shadow: 0 8px 20px rgba(255, 215, 0, 0.4);
  transition: transform 0.3s ease;
  position: relative;
  overflow: hidden;

  &::before {
    content: "";
    position: absolute;
    top: -50%;
    left: -50%;
    width: 200%;
    height: 200%;
    background: radial-gradient(
      circle,
      rgba(255, 255, 255, 0.3) 0%,
      transparent 70%
    );
    animation: sparkle 2s ease-in-out infinite;
  }

  &:hover {
    transform: scale(1.1);
  }
}

@keyframes sparkle {
  0%,
  100% {
    opacity: 0;
    transform: rotate(0deg);
  }
  50% {
    opacity: 1;
    transform: rotate(180deg);
  }
}

.barcode {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  height: 60px;
  padding: 0 10px;
  margin: 20px 0;
}

.bar {
  background: linear-gradient(to top, #ffffff, #cccccc);
  width: 3px;
  border-radius: 1px;
  opacity: 0.8;
}

.ticket-footer {
  padding: 20px;
  text-align: center;
  border-top: 2px dashed rgba(255, 255, 255, 0.2);
  background: rgba(0, 0, 0, 0.2);
}

.action-btn {
  background: linear-gradient(135deg, #4ecdc4, #44a08d);
  color: #ffffff;
  border: none;
  padding: 15px 30px;
  font-size: 1.1rem;
  font-weight: bold;
  border-radius: 50px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 10px 30px rgba(78, 205, 196, 0.3);
  position: relative;
  overflow: hidden;

  &::before {
    content: "";
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(
      90deg,
      transparent,
      rgba(255, 255, 255, 0.2),
      transparent
    );
    transition: left 0.5s ease;
  }

  &:hover {
    transform: translateY(-2px);
    box-shadow: 0 15px 40px rgba(78, 205, 196, 0.4);

    &::before {
      left: 100%;
    }
  }
}

.features {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 30px;
  margin: 60px 0;
}

.feature-card {
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(20px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 20px;
  padding: 30px;
  text-align: center;
  transition: transform 0.3s ease;

  &:hover {
    transform: translateY(-10px);
  }
}

.feature-icon {
  font-size: 3rem;
  color: #ffd700;
  margin-bottom: 20px;
}

.feature-title {
  color: #ffffff;
  font-size: 1.3rem;
  font-weight: bold;
  margin-bottom: 15px;
}

.feature-text {
  color: rgba(255, 255, 255, 0.8);
  line-height: 1.6;
}

// Адаптивные стили
@media (max-width: 768px) {
  .container {
    padding: 10px;
  }

  .logo {
    font-size: 2rem;
  }

  .banner {
    padding: 20px;
  }

  .jackpot-amount {
    font-size: 2.5rem;
  }

  .countdown {
    flex-wrap: wrap;
    gap: 10px;
  }

  .countdown-item {
    padding: 15px;
    min-width: 60px;
  }

  .numbers {
    grid-template-columns: repeat(3, 1fr);
  }
}
</style>
