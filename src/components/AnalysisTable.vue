<template>
  <div class="p-6">
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
            <div class="flex gap-2 flex-wrap">
              <img
                v-for="(photo, idx) in item.photos"
                :key="idx"
                :src="photo"
                alt="анализ"
                class="w-24 h-auto border"
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
  </div>
</template>

<script setup>
import analyses from "../data/analyses.json";
</script>

<style scoped>
table {
  border-collapse: collapse;
}
</style>
