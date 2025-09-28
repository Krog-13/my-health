
<template>
  <div class="p-6 relative">
    <h2 class="text-xl font-bold mb-4">История приёмов</h2>
    <table class="w-full border border-gray-300">
      <thead>
        <tr class="bg-gray-100">
          <th class="border p-2">Дата приёма</th>
          <th class="border p-2">Диагноз</th>
          <th class="border p-2">Симптомы</th>
          <th class="border p-2">Фото анализов</th>
          <th class="border p-2">Метод лечения</th>
          <th class="border p-2">Доктор</th>
          <th class="border p-2">Должность</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in analyses" :key="index">
          <td class="border p-2">{{ item.date }}</td>
          <td class="border p-2">{{ item.diagnosis }}</td>
          <td class="border p-2">{{ item.symptoms }}</td>
          <td class="border p-2">
            <div class="flex flex-row gap-2 overflow-x-auto">
              <span v-if="!item.photos || item.photos.length === 0">Нет фото</span>
              <img
                v-else
                v-for="(photo, idx) in item.photos"
                :key="idx"
                :src="photo"
                alt="Анализ"
                class="thumbnail cursor-pointer"
                @click.stop="openModal(photo)"
                @error="handleImageError"
              />
            </div>
          </td>
          <td class="border p-2">
            <ul class="list-disc pl-5">
              <li v-for="(treat, tIndex) in item.treatment" :key="tIndex">
                {{ treat }}
              </li>
            </ul>
          </td>
          <td class="border p-2">{{ item.doctor }}</td>
          <td class="border p-2">{{ item.position }}</td>
        </tr>
      </tbody>
    </table>

    <!-- Модальное окно -->
    <transition name="fade">
      <div
        v-if="selectedPhoto"
        class="fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center z-50"
        @click="closeModal"
      >
        <div class="relative flex items-center justify-center" @click.stop>
          <button
            class="absolute top-2 right-2 text-white bg-gray-800 rounded-full p-2 hover:bg-gray-600 z-10"
            @click.stop="closeModal"
            aria-label="Закрыть модальное окно"
          >
            ✕
          </button>
          <div
            class="image-container"
            @wheel.prevent="handleZoom"
            @mousedown="startDragging"
            @mousemove="dragImage"
            @mouseup="stopDragging"
            @mouseleave="stopDragging"
          >
            <img
              :src="selectedPhoto"
              alt="Анализ"
              class="modal-image"
              :style="imageStyle"
              @dragstart.prevent
            />
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from "vue";
import analyses from "../data/analyses.json";

const selectedPhoto = ref(null);
const scale = ref(1); // Масштаб изображения
const translateX = ref(0); // Смещение по X
const translateY = ref(0); // Смещение по Y
const isDragging = ref(false);
const startX = ref(0);
const startY = ref(0);
const lastX = ref(0);
const lastY = ref(0);

const imageStyle = computed(() => ({
  transform: `scale(${scale.value}) translate(${translateX.value}px, ${translateY.value}px)`,
  cursor: "move", // Курсор всегда "move" для перетаскивания
}));

function openModal(photo) {
  selectedPhoto.value = photo;
  scale.value = 1; // Сбрасываем масштаб
  translateX.value = 0; // Сбрасываем смещение
  translateY.value = 0; // Сбрасываем смещение
  document.body.style.overflow = "hidden"; // Блокируем прокрутку фона
  document.body.style.pointerEvents = "none"; // Блокируем взаимодействие с фоном
  document.body.classList.add("modal-open"); // Добавляем класс для блокировки выделения
  document.querySelector(".fixed").style.pointerEvents = "auto"; // Разрешаем взаимодействие с модалкой
}

function closeModal() {
  selectedPhoto.value = null;
  scale.value = 1;
  translateX.value = 0;
  translateY.value = 0;
  document.body.style.overflow = ""; // Восстанавливаем прокрутку
  document.body.style.pointerEvents = ""; // Восстанавливаем взаимодействие
  document.body.classList.remove("modal-open"); // Удаляем класс
}

function handleImageError(event) {
  event.target.src = "/placeholder.jpg"; // Путь к заглушке
}

function handleZoom(event) {
  event.preventDefault();
  const zoomStep = 0.1;
  const minScale = 1;
  const maxScale = 3;

  if (event.deltaY < 0) {
    // Увеличение
    scale.value = Math.min(scale.value + zoomStep, maxScale);
  } else {
    // Уменьшение
    scale.value = Math.max(scale.value - zoomStep, minScale);
  }
}

function startDragging(event) {
  isDragging.value = true;
  startX.value = event.clientX - lastX.value;
  startY.value = event.clientY - lastY.value;
}

function dragImage(event) {
  if (isDragging.value) {
    translateX.value = event.clientX - startX.value;
    translateY.value = event.clientY - startY.value;
    lastX.value = translateX.value;
    lastY.value = translateY.value;
  }
}

function stopDragging() {
  isDragging.value = false;
}

function handleEscape(event) {
  if (event.key === "Escape" && selectedPhoto.value) {
    closeModal();
  }
}

onMounted(() => {
  window.addEventListener("keydown", handleEscape);
});

onUnmounted(() => {
  window.removeEventListener("keydown", handleEscape);
});
</script>

<style scoped>
table {
  border-collapse: collapse;
}

.thumbnail {
  width: 60px;
  height: 60px;
  object-fit: cover;
  border: 1px solid #ccc;
  border-radius: 4px;
  transition: transform 0.2s;
}

.thumbnail:hover {
  transform: scale(1.05);
}

/* Фон модалки */
.fixed.inset-0 {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
  pointer-events: auto;
}

/* Контейнер для картинки */
.image-container {
  max-width: 90vw;
  max-height: 90vh;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

/* Картинка */
.modal-image {
  max-width: 100%;
  max-height: 90vh;
  object-fit: contain;
  display: block;
  margin: auto;
  user-select: none;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

/* Блокируем выделение текста на странице, когда модалка открыта */
body.modal-open * {
  user-select: none;
}
</style>