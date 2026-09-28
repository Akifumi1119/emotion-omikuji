<script setup lang="ts">
import { ref } from "vue";
import img0 from "./assets/image0.jpeg";
import img1 from "./assets/image1.jpeg";
import img2 from "./assets/image2.jpeg";
import img3 from "./assets/image3.jpeg";

const images = [img0, img1, img2, img3];
const revealed = ref(false);
const shaking = ref(false);
const currentImage = ref("");

function draw() {
  if (shaking.value) return;
  shaking.value = true;
  revealed.value = false;

  setTimeout(() => {
    const idx = Math.floor(Math.random() * images.length);
    currentImage.value = images[idx];
    shaking.value = false;
    revealed.value = true;
  }, 600);
}
</script>

<template>
  <div class="page">
    <header class="header">
      <div class="header-inner">
        <h1 class="title-ja">味覚おみくじ</h1>
      </div>
    </header>

    <main class="main">
      <div class="card-area">
        <transition name="flip">
          <div
            v-if="!revealed"
            class="card card--back"
            :class="{ shaking }"
            @click="draw"
          >
            <div class="card-back-inner">
              <div class="torii">👶</div>
              <p class="card-hint">タップして♡</p>
            </div>
          </div>
          <div v-else class="card card--front">
            <img :src="currentImage" alt="おみくじ結果" class="result-img" />
          </div>
        </transition>
      </div>

      <div v-if="revealed" class="actions">
        <!-- <button v-if="!revealed" class="btn btn--draw" @click="draw">
          <span class="btn-text">今日の運勢を見る</span>
        </button> -->
        <button class="btn btn--retry" @click="draw">もう一度引く</button>
      </div>
    </main>

    <footer class="footer">© 2026 baby-omikuji</footer>
  </div>
</template>

<style scoped>
.page {
  min-height: 100dvh;
  display: flex;
  flex-direction: column;
  align-items: center;
  background: linear-gradient(160deg, #1a0a00 0%, #3b1505 50%, #1a0a00 100%);
  color: #f5e6c8;
  font-family: "Hiragino Mincho ProN", "Yu Mincho", serif;
}

/* Header */
.header {
  width: 100%;
  padding: 2rem 0 1.5rem;
  text-align: center;
  border-bottom: 1px solid rgba(245, 200, 120, 0.25);
}
.header-inner {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.25rem;
}
.title-en {
  font-size: 0.75rem;
  letter-spacing: 0.4em;
  color: #c8973a;
  font-family: "Georgia", serif;
}
.title-ja {
  font-size: 2rem;
  letter-spacing: 0.3em;
  font-weight: 700;
  margin: 0;
  background: linear-gradient(135deg, #f5c842, #e8960a);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

/* Main */
.main {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 2.5rem;
  padding: 2rem 1rem;
}

/* Card */
.card-area {
  perspective: 900px;
  width: min(340px, 88vw);
  height: min(440px, 110vw);
  position: relative;
}
.card {
  width: 100%;
  height: 100%;
  border-radius: 18px;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow:
    0 20px 60px rgba(0, 0, 0, 0.6),
    0 0 30px rgba(200, 151, 58, 0.15);
}
.card--back {
  background: linear-gradient(145deg, #5c2200 0%, #2a0d00 100%);
  border: 1px solid rgba(200, 151, 58, 0.4);
  cursor: pointer;
}
.card--back:hover {
  border-color: rgba(200, 151, 58, 0.8);
  box-shadow:
    0 20px 60px rgba(0, 0, 0, 0.6),
    0 0 50px rgba(200, 151, 58, 0.3);
}
.card-back-inner {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.5rem;
}
.torii {
  font-size: 4rem;
  filter: drop-shadow(0 0 12px rgba(200, 151, 58, 0.7));
}
.card-hint {
  font-size: 0.85rem;
  letter-spacing: 0.1em;
  color: #c8973a;
  margin: 0;
}

.card--front {
  flex-direction: column;
  gap: 0;
  overflow: hidden;
  background: #1a0a00;
  border: 2px solid rgba(200, 151, 58, 0.6);
  position: relative;
}
.result-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

/* Shake animation */
@keyframes shake {
  0% {
    transform: rotate(0deg);
  }
  20% {
    transform: rotate(-6deg) translateY(-4px);
  }
  40% {
    transform: rotate(6deg) translateY(-8px);
  }
  60% {
    transform: rotate(-4deg) translateY(-4px);
  }
  80% {
    transform: rotate(4deg);
  }
  100% {
    transform: rotate(0deg);
  }
}
.shaking {
  animation: shake 0.6s ease-in-out;
}

/* Flip transition */
.flip-enter-active,
.flip-leave-active {
  transition:
    opacity 0.3s ease,
    transform 0.4s ease;
  position: absolute;
  width: 100%;
  height: 100%;
}
.flip-enter-from {
  opacity: 0;
  transform: rotateY(-90deg) scale(0.9);
}
.flip-leave-to {
  opacity: 0;
  transform: rotateY(90deg) scale(0.9);
}

/* Buttons */
.actions {
  display: flex;
  justify-content: center;
}
.btn {
  padding: 0.9rem 2.8rem;
  border-radius: 999px;
  font-family: inherit;
  font-size: 1rem;
  letter-spacing: 0.15em;
  font-weight: 600;
  cursor: pointer;
  border: none;
  transition:
    transform 0.15s,
    box-shadow 0.15s;
}
.btn:active {
  transform: scale(0.96);
}
.btn--draw {
  background: linear-gradient(135deg, #f5c842, #c8730a);
  color: #1a0a00;
  box-shadow: 0 6px 24px rgba(200, 115, 10, 0.45);
}
.btn--draw:hover {
  box-shadow: 0 8px 30px rgba(200, 115, 10, 0.65);
  transform: translateY(-2px);
}
.btn--retry {
  background: transparent;
  color: #c8973a;
  border: 1.5px solid #c8973a;
}
.btn--retry:hover {
  background: rgba(200, 151, 58, 0.1);
}

/* Footer */
.footer {
  width: 100%;
  text-align: center;
  padding: 1rem;
  font-size: 0.75rem;
  color: rgba(200, 151, 58, 0.4);
  letter-spacing: 0.1em;
  border-top: 1px solid rgba(245, 200, 120, 0.1);
}
</style>
