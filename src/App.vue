<script setup lang="ts" >
import { ref, onMounted } from "vue";
const arrImg = ref(["/hoadat/src/assets/3.JPG", "/hoadat/src/assets/2.JPG"]);
const flowerId = ref(0);
const flowers = ref([]);
const day_names = ref(["T2", "T3", "T4", "T5", "T6", "T7", "CN"]);
const month_names = ref([
  "Tháng 1",
  "Tháng 2",
  "Tháng 3",
  "Tháng 4",
  "Tháng 5",
  "Tháng 6",
  "Tháng 7",
  "Tháng 8",
  "Tháng 9",
  "Tháng 10",
  "Tháng 11",
  "Tháng 12",
]);
const showIndex = ref(0);
const flower = ref({}) as any;
function startFlowerFall() {
  setInterval(() => {
    flowers.value.push(createFlower());
    // Giới hạn số lượng hoa để không làm chậm trang
    if (flowers.value.length > 100) {
      flowers.value.shift();
    }
  }, 1000); // Tạo hoa mới mỗi 500ms
  setInterval(() => {
    showIndex.value++;

    if (showIndex.value >= arrImg.value.length) {
      showIndex.value = 0;
    }
  }, 2000); // chuyển slide mỗi 1s
}
function createFlower() {
  flower.value = {
    id: flowerId.value++,
    x: Math.random() * window.innerWidth,
    duration: Math.random() * 5 + 10, // Thời gian rơi từ 5 đến 10 giây
  };
  return flower.value;
}
function createFlowers() {
  for (let i = 0; i < 50; i++) {
    // Số lượng hoa rơi ban đầu
    flowers.value.push(createFlower());
  }
}
function namNhuan(year: number) {
  return (
    (year % 4 === 0 && year % 100 !== 0 && year % 400 !== 0) ||
    (year % 100 === 0 && year % 400 === 0)
  );
}
function daysNamNhuan(year: number) {
  return namNhuan(year) ? 29 : 28;
}
function daysgenerater() {
  let days: any = [];
  for (let k = 0; k < days_of_month().length; k++) {
    days.push([]);
    for (let i = 1; i <= days_of_month()[k]; i++) {
      if (days[k].length < hafta(new Date().getFullYear(), k)) {
        i -= i;
        days[k].push("");
        continue;
      }
      days[k].push(i);
    }
  }
  return days;
}
function days_of_month() {
  return [
    31,
    daysNamNhuan(new Date().getFullYear()),
    31,
    30,
    31,
    30,
    31,
    31,
    30,
    31,
    30,
    31,
  ];
}
function isToDay(kun: any, index: number) {
  const dayintable = new Date(new Date().getFullYear(), index, kun);

  return dayintable.getDate() == 1 || dayintable.getDate() == 2 ? true : false;
}
function hafta(sol: any, ma: any) {
  let haftakuni = new Date(sol, ma).getDay();
  switch (
    haftakuni // hafta kuni Dushanbadan boshlangani uchun hak)
  ) {
    case 0:
      haftakuni = 6;
      break;
    case 1:
      haftakuni = 0;
      break;
    case 2:
      haftakuni = 1;
      break;
    case 3:
      haftakuni = 2;
      break;
    case 4:
      haftakuni = 3;
      break;
    case 5:
      haftakuni = 4;
      break;
    case 6:
      haftakuni = 5;
      break;
    default:
      haftakuni = new Date(sol, ma).getDay();
      break;
  }
  return haftakuni;
}
onMounted(() => {
  createFlowers();
  startFlowerFall();
});
</script>
<template>
  <div class="h-screen overflow-y-scroll">
    <div
      v-for="flower in flowers"
      :key="flower.id"
      class="flower"
      :style="{
        left: flower.x + 'px',
        animationDuration: flower.duration + 's',
      }"
    >
      🌸
    </div>
    <div class="w-full flex justify-center">
      <img src="./assets/title.png" alt="" />
    </div>
    <div class="w-full md:flex md:justify-center">
      <div class="w-full md:w-4/5" v-for="(img, index) in arrImg" :key="index">
        <img :src="img" alt="" v-if="showIndex == index" />
      </div>
    </div>
    <div class="flex flex-col md:flex-row md:px-4">
      <div class="flex justify-center md:justify-end">
        <div class="text-center">
          <p class="text-3xl md:text-7xl">Save the Date</p>
          <p class="text-xl md:text-lg">For the weddiing of</p>
          <p class="text-2xl md:text-4xl">Công Đạt & Thanh Hoa</p>
          <p>
            Thật vui vì được gặp và đón tiếp mọi trong một dịp đặc biệt như này.
            Cảm ơn các mọi người rất nhiều vì sự hiện diện cùng những lời chúc
            tốt đẹp mà mọi người đã dành cho gia đình và chúng em/anh/chị/cháu!
          </p>
        </div>
      </div>
      <div class="flex justify-center md:justify-start mt-4">
        <div class="w-full">
          <div
            class="text-center w-full"
            v-for="(month, index) of month_names"
            :key="index"
          >
            <div
              v-show="index == 5"
              class="p-1 m-1 font-sans bg-white w-full min-h-56 rounded shadow-md bg-blend-luminosity bg-gradient-to-b from-green-50 via-white to-green-50"
            >
              <p class="p-1 text-xl font-semibold text-center text-indigo-800">
                {{ month }}-2024
              </p>
              <div class="p-1 m-1">
                <div
                  class="grid grid-cols-7 font-semibold text-green-800 border-b-2"
                >
                  <div v-for="days in day_names" :key="days">
                    <div
                      class="grid place-items-center"
                      :class="{
                        'text-red-600': days == 'CN',
                      }"
                    >
                      <p class="text-2xl">{{ days }}</p>
                    </div>
                  </div>
                </div>

                <div
                  class="grid grid-cols-7 gap-1 font-semibold text-center text-gray-800"
                >
                  <div v-for="kun of daysgenerater()[index]" :key="kun">
                    <div
                      :class="{
                        ' bg-pink-200 ring-4 rounded-full':
                          isToDay(kun, index) == true,
                      }"
                    >
                      <p class="text-2xl">{{ kun }}</p>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="flex flex-col md:flex-row md:px-4 mt-4">
      <div class="flex justify-center md:justify-end">
        <div class="w-full text-center">
          <div class="font-bold text-2xl">Tiệc nhà Trai</div>
          <img src="/hoadat/src/assets/trai.jpg" alt="" />
        </div>
      </div>
      <div class="flex justify-center md:justify-start mt-4">
        <div class="w-full text-center">
          <div class="font-bold text-2xl">Tiệc nhà Gái</div>
          <img src="/hoadat/src/assets/gai.jpg" alt="" />
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.flower {
  position: absolute;
  top: -50px; /* Bắt đầu từ trên màn hình */
  font-size: 14px;
  animation-name: fall;
  animation-timing-function: linear;
}

@keyframes fall {
  0% {
    transform: translateY(0);
  }
  100% {
    transform: translateY(100vh); /* Rơi xuống cuối màn hình */
  }
}
</style>
