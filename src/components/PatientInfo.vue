<template>
  <div class="p-6 bg-blue-50 rounded-xl mb-6">
    <h2 class="text-xl font-bold mb-4">О себе</h2>
    <div class="grid grid-cols-2 gap-4">
      <div><strong>Имя:</strong> {{ patient.name }}</div>
      <div><strong>Возраст:</strong> {{ patient.age }} лет</div>
      <div><strong>Город:</strong> {{ patient.city }}</div>
      <div><strong>Рост:</strong> {{ patient.height }} см</div>
      <div><strong>Вес:</strong> {{ patient.weight }} кг</div>
      <div><strong>Курение:</strong> {{ patient.smoking }}</div>
      <div><strong>Алкоголь:</strong> {{ patient.alcohol }}</div>
      <div><strong>Место работы:</strong> {{ patient.workplace }}</div>
      <div><strong>Должность:</strong> {{ patient.position }}</div>
      <div><strong>Семейное положение:</strong> {{ patient.marital_status }}</div>
      <div><strong>Режим питания:</strong> {{ patient.diet }}</div>
      <div class="col-span-1 md:col-span-2">
        <hr class="my-2">
        <strong>Основное питание:</strong>
        <ul>
          <li><strong class="text-gray-700">Завтрак:</strong> {{ patient.main_food.breakfast }}</li>
          <li><strong class="text-gray-700">Обед:</strong> {{ patient.main_food.lunch }}</li>
          <li><strong class="text-gray-700">Ужин:</strong> {{ patient.main_food.dinner }}</li>
          <li><strong class="text-gray-700">Перекусы:</strong> {{ patient.main_food.snacks }}</li>
        </ul>
        <hr class="my-2">
      </div>
    </div>

    <div v-if="patient.additional" class="mt-4">
      <strong>Дополнительно:</strong>
      <p class="mt-1 bg-white p-3 rounded border">{{ patient.additional }}</p>
    </div>
  </div>
</template>

<script setup>
import patient from "../data/patient.json";
</script>
