<template>
  <div class="p-6 bg-blue-50 rounded-xl mb-6">
    <h2 class="text-xl font-bold mb-4">О себе</h2>
    <div class="grid grid-cols-2 gap-4">
      <div><strong>Имя:</strong> {{ patient.name }}</div>
      <div><strong>Город:</strong> {{ patient.city }}</div>
      <div><strong>Рост:</strong> {{ patient.height }} см</div>
      <div><strong>Вес:</strong> {{ patient.weight }} кг</div>
      <div><strong>Курение:</strong> {{ patient.smoking }}</div>
      <div><strong>Алкоголь:</strong> {{ patient.alcohol }}</div>
      <div><strong>Место работы:</strong> {{ patient.workplace }}</div>
      <div><strong>Должность:</strong> {{ patient.position }}</div>
      <div><strong>Режим питания:</strong> {{ patient.diet }}</div>
      <div><strong>Основное питание:</strong> {{ patient.main_food }}</div>
      <div><strong>Основное питание:</strong> {{ patient.marital_status }}</div>
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
