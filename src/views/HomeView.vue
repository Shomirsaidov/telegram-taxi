<template>
  <div class="bg-white  shadow-lg max-w-md mx-auto">
    <!-- Header -->
    <!-- <div class="flex items-center justify-between p-4">
      <h1 class="text-lg font-semibold text-blue-500">Telegram Taxi</h1>
      <button class="text-gray-400 hover:text-gray-600">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
        </svg>
      </button>
    </div> -->

    <!-- Map Section -->
    <div class="relative h-80">


      <!-- <div class="h-48 bg-gray-200"></div>
      <div class="absolute top-4 left-4 bg-white shadow p-2 rounded-md">
        
        <p class="text-sm">просп. Добровольского, 111</p>
      </div>
      <div class="absolute bottom-4 left-4 bg-white shadow p-2 rounded-md">
        <p class="text-sm">Супермаркет Ашан</p>
      </div> -->

      <l-map
        class="w-full map"
        :zoom="23"
        :center="[46.482526, 30.723309]"]
      >
        <l-tile-layer :url="tileLayerUrl" :attribution="tileLayerAttribution" />
        <!-- Markers -->
        <l-marker :lat-lng="[46.482526, 30.723309]">
          <l-popup>Проспект Добровольского, 111</l-popup>
        </l-marker>
        <l-marker :lat-lng="[46.479428, 30.707328]">
          <l-popup>Супермаркет Ашан</l-popup>
        </l-marker>
      </l-map>
    </div>


    <div class="flex flex-col justify-between h-full">

      <div class="my-bg rounded-t-xl mt-0 p-4 pb-40">

      <!-- Address Fields -->
      <div class="mt-4 space-y-2">
        <div class="flex items-center space-x-3 px-3 py-3 border rounded-md text-sm shadow-sm  bg-white">
          <img src="../assets/target.svg" alt="">
          <input type="text" value="Проспект Добровольского, 111" class="w-full outline-none" />
        </div>
        <div class="flex items-center space-x-3 px-3 py-2 border rounded-md text-sm shadow-sm bg-white">
          <img src="../assets/location.svg" alt="">
          <input type="text" value="Супермаркет Ашан" class="w-full outline-none" />
        </div>
      </div>

      <!-- Transport Options -->
      <!-- Transport Options -->
<div class="mt-4 grid grid-cols-2 gap-2">
  <button
    v-for="(option, index) in transportOptions"
    :key="index"
    @click="selectOption(index)"
    :class="[
      'p-2 border rounded-md shadow',
      selectedOption === index ? 'my-blue text-white' : 'bg-white'
    ]"
  >
    <div class="flex items-center justify-between">
      <div class="flex items-center space-x-2">
        <img :src="option.icon" alt="" />
        <div :class="selectedOption === index ? 'text-lgg font-semibold' : 'text-lgg'">
          {{ option.label }}
        </div>
      </div>
      <div>
        <input
          type="checkbox"
          :checked="selectedOption === index"
          class="h-6 w-6 text-blue-600 rounded focus:ring focus:ring-blue-200"
          readonly
        />
      </div>
    </div>
    <div v-if="option.details" class="grid grid-cols-2 space-x-2 mt-2 w-full">
      <div>
        <div class="text-start text-xss">Сумма</div>
        <div
          :class="[
            'rounded text-mdd w-full',
            selectedOption === index ? 'bg-white text-my-blue font-bold' : 'bg-blue-200'
          ]"
        >
          {{ option.details.sum }}
        </div>
      </div>
      <div>
        <div class="text-start text-xss">Время поездки</div>
        <div
          :class="[
            'rounded text-mdd w-full',
            selectedOption === index ? 'bg-white text-my-blue font-bold' : 'bg-blue-200'
          ]"
        >
          {{ option.details.time }}
        </div>
      </div>
    </div>
    <div v-else class="text-start text-xss"><i>{{ option.note }}</i></div>
  </button>
</div>


      <div class="mt-3">
          <textarea placeholder="примечания к заказу" class="w-full px-3 py-2 rounded-md text-sm focus:ring focus:ring-blue-200 focus:outline-none"></textarea>
      </div>

      <!-- Footer Buttons -->
      


      </div>


      <div class="fixed bottom-0 w-full flex space-x-2 font-semibold my-bg p-4">
        <button class="flex-1 p-2 text-sm text-white border shadow-xl rounded-xl hover:bg-blue-100 my-blue-2">
          Предложить свою цену
        </button>
        <button class="flex-1 p-2 text-sm text-white  rounded-xl shadow-xl hover:bg-blue-600 my-blue">
          Подтвердить заказ
        </button>
        <button class="flex-1 p-2 text-sm text-white  rounded-xl shadow-xl hover:bg-red-600 my-red">
          Отменить заказ
        </button>
      </div>

    </div>

  </div>
</template>

<script>

import { LMap, LTileLayer, LMarker, LPopup } from "vue3-leaflet";

export default {
  name: "TaxiOrderComponent",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
  },
  data() {
    return {
      selectedOption: null,
      tileLayerUrl: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      tileLayerAttribution:
        '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
        transportOptions: [
        {
          label: "Такси",
          icon: require("../assets/car.svg"),
          details: { sum: "234 грн", time: "15 мин" },
        },
        {
          label: "Частный водитель",
          icon: require( "../assets/private.svg"),
          details: { sum: "234 грн", time: "15 мин" },
        },
        {
          label: "Мотоцикл",
          icon: require("../assets/moto.svg"),
          details: { sum: "234 грн", time: "15 мин" },
        },
        {
          label: "Главное быстро",
          icon: require("../assets/clock.svg"),
          note: "Отметить все виды транспорта",
        },
      ],
    };
  },
  methods: {
    selectOption(index) {
      this.selectedOption = index; // Update selected button index
    },
  },
};
</script>

<style scoped>

.map {
  min-height: 400px;
}

.my-bg {
  background: #EFEFF3;
}

.my-blue-2 {
  background: #89AED0;
}

.my-red {
  background: #E85D5D;
}

.my-blue {
  background: #52A2EB;
}

.text-xss {
 font-size: 9px;
}


.text-mdd {
 font-size: 13px;
}

.text-lgg {
 font-size: 14px;
}

.text-my-blue {
  color: #52A2EB;
}


</style>
