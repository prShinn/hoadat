<script setup lang="ts" >
import { ref, onMounted } from "vue";
import confetti from "canvas-confetti";
const arrImg = ref([
  "/hoadat/src/assets/6b63aa212102815cd813.jpg",
  "/hoadat/src/assets/b01eb2433960993ec071.jpg",
]);
const flowerId = ref(0);
const flowers = ref<any[]>([]);
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
function startFireWork() {
  const duration = 0.3 * 1000;
  const end = Date.now() + duration;

  const colors = ["#bb0000", "#ffffff", "#FFC0CB"];
  (function frame() {
    confetti({
      particleCount: 3,
      angle: 60,
      spread: 55,
      origin: { x: 0 },
      colors: colors,
    });
    confetti({
      particleCount: 3,
      angle: 120,
      spread: 55,
      origin: { x: 1 },
      colors: colors,
    });

    if (Date.now() < end) {
      requestAnimationFrame(frame);
    }
  })();
}
function startFlowerFall() {
  setInterval(() => {
    if (!flowers.value) {
      flowers.value = [];
    }
    flowers.value.push(createFlower());
    // Giới hạn số lượng hoa để không làm chậm trang
    if (flowers.value.length > 50) {
      flowers.value.shift();
    }
  }, 1000); // Tạo hoa mới mỗi 500ms
  setInterval(() => {
    showIndex.value++;

    if (showIndex.value >= arrImg.value.length) {
      showIndex.value = 0;
    }
  }, 3000); // chuyển slide mỗi 1s
  setInterval(() => {
    startFireWork();
  }, 5000);
}
function createFlower() {
  return {
    id: flowerId.value++,
    x: Math.random() * window.innerWidth,
    duration: Math.random() * 5 + 10, // Thời gian rơi từ 5 đến 10 giây
  };
}
function createFlowers() {
  if (!flowers.value) {
    flowers.value = [];
  }
  for (let i = 0; i < 5; i++) {
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
  <div class="h-screen overflow-y-scroll bg-img">
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
    <!-- Title -->
    <div class="w-full flex justify-center">
      <!-- <img src="./assets/title.png" alt="" /> -->
      <img src="./assets/banner.jpg" alt="" />
    </div>
    <p class="text-center font-comic text-pink-400 text-xs bg-pink-100">
      chúng mình cưới đây !!!
    </p>
    <!-- Ảnh cưới -->
    <div class="flex justify-center">
      <div
        class="img-slide flex"
        v-show="showIndex == 0"
        :style="{
          left: 0,
          animationDuration: '1s',
        }"
      >
        <img class="w-full" src="./assets/b01eb2433960993ec071.jpg" alt="" />
        <img class="w-full" src="./assets/6b63aa212102815cd813.jpg" alt="" />
      </div>
      <div
        class="img-slide flex"
        v-show="showIndex == 1"
        :style="{
          left: 0,
          animationDuration: '1s',
        }"
      >
        <img class="w-full" src="./assets/6b63aa212102815cd813.jpg" alt="" />
        <img class="w-full" src="./assets/b01eb2433960993ec071.jpg" alt="" />
      </div>
    </div>
    <!-- Lời mời + ngày cưới -->
    <div class="flex flex-col md:flex-row md:gap-8 p-4 bg-img">
      <div class="flex justify-center md:justify-end">
        <div class="text-center border-4 rounded-xl p-4 md:w-3/4">
          <p class="text-3xl md:text-7xl font-black font-brush">
            Save the Date
          </p>
          <p class="text-xl md:text-lg font-brush">For the weddiing of</p>
          <p class="text-2xl md:text-4xl font-comic">Công Đạt ♥ Thanh Hoa</p>
          <p class="mt-2 font-sans">
            Thật vui vì được gặp và đón tiếp mọi trong một dịp đặc biệt như này.
            Cảm ơn mọi người rất nhiều vì sự hiện diện cùng những lời chúc tốt
            đẹp mà mọi người đã dành cho gia đình và chúng em/anh/chị/cháu!
          </p>
        </div>
      </div>
      <div
        class="w-full flex justify-center md:gap-4 md:justify-start mt-8 md:mt-0"
      >
        <div class="w-full md:w-3/4 border-2 rounded-lg p-4">
          <!-- <p class="text-center text-3xl font-extrabold font-itim">
            Ngày tổ chức
          </p> -->
          <div class="text-center w-full">
            <img src="./assets/date.jpg" alt="" />
          </div>
          <!-- <div
            class="text-center w-full"
            v-for="(month, index) of month_names"
            :key="index"
          >
            <div
              v-show="index == 5"
              class="p-1 m-1 font-sans w-full min-h-56 rounded"
            >
              <p
                class="p-1 text-xl font-semibold text-center text-indigo-800 font-brush"
              >
                {{ month }}-2024
              </p>
              <div class="p-1 m-1">
                <div
                  class="grid grid-cols-7 font-semibold text-green-800 border-b-2 mb-2"
                >
                  <div v-for="days in day_names" :key="days">
                    <div
                      class="grid place-items-center font-comic"
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
                        ' bg-pink-200 rounded-full p-2':
                          isToDay(kun, index) == true,
                      }"
                    >
                      <p class="text-2xl font-sans">{{ kun }}</p>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div> -->
        </div>
      </div>
    </div>
    <div class="border-b-2 my-2"></div>
    <!-- Thông tin cô dâu + chú rể -->
    <div class="w-full my-2 bg-img">
      <img class="w-full" src="./assets/cdcr.jpg" alt="" />
    </div>
    <div class="flex flex-col md:flex-row md:gap-4 px-4 mt-4">
      <div class="w-full flex justify-center md:justify-end items-center gap-4">
        <div class="w-full border-r-4 rounded-full">
          <img class="rounded-full" src="./assets/DSC03105.jpg" alt="" />
        </div>
        <div>
          <p class="text-center text-2xl font-itim">Chú rể</p>
          <p class="font-sans">
            Anh là chàng trai mang hài xanh, dành hết thanh xuân để yêu em !!!
          </p>
        </div>
      </div>
      <div
        class="w-full flex justify-center md:gap-4 md:justify-start mt-8 md:mt-0"
      >
        <div class="w-full flex items-center gap-4">
          <div>
            <p class="text-center text-2xl font-itim">Cô dâu</p>
            <p class="font-sans">
              Em là cô gái mang hài đỏ, bỏ thế giới nhỏ để bên anh !!!
            </p>
          </div>
          <div class="w-full border-r-4 rounded-full">
            <img
              class="rounded-full"
              src="./assets/73b058e59cc63c9865d7.jpg"
              alt=""
            />
          </div>
        </div>
      </div>
    </div>
    <!-- Sự kiện cưới -->
    <div class="w-full border-t-4 pt-6 mt-4 bg-[#f3f3f3] bg-img">
      <div class="text-center text-3xl font-extrabold font-sans">
        Sự Kiện Cưới
      </div>
      <div class="flex flex-col md:flex-row md:px-4 mt-4">
        <div class="flex justify-center md:justify-start mt-4">
          <div class="w-3/4 bg-white px-2 py-10 text-center">
            <div class="font-bold text-2xl font-itim">Lễ cưới nhà Gái</div>
            <div class="flex justify-center my-1">
              <img
                class="rounded-md w-1/2"
                src="./assets/64b5ecb0289388cdd182.jpg"
                alt=""
              />
            </div>
            <div class="font-itim text-xl my-1 px-4">
              số 3 ngách 87/43/10 yên xá tân triều hà nội
            </div>
            <div class="flex justify-center">
              <a
                class="border-2 w-3/4 p-2 my-1 rounded-full text-blue-500 font-bold"
                href="https://maps.app.goo.gl/YBpMKKNBBY5mAZocA"
              >
                Xem bản đồ</a
              >
            </div>
          </div>
        </div>
        <div class="flex justify-center md:justify-start mt-4 md:mt-0">
          <div class="w-3/4 bg-white px-2 py-10 text-center p-4 border">
            <div class="font-bold text-2xl font-itim">Tiệc cưới nhà Gái</div>
            <div class="flex justify-center my-1">
              <img
                class="rounded-md w-1/2"
                src="./assets/73b058e59cc63c9865d7.jpg"
                alt=""
              />
            </div>
            <div class="font-itim text-xl my-1 px-4">
              Sân chơi cộng đồng yên xá
            </div>
            <div class="flex justify-center">
              <a
                class="border-2 w-3/4 p-2 my-1 rounded-full text-blue-500 font-bold"
                href="https://maps.app.goo.gl/EohrdVRQ5mG8enZu6"
              >
                Xem bản đồ</a
              >
            </div>
          </div>
        </div>
        <div
          class="flex justify-center md:justify-end mt-6 md:mt-0 border-t-4 pt-6"
        >
          <div class="w-3/4 bg-white px-2 py-10 text-center p-4 border">
            <div class="font-bold text-2xl font-itim">Lễ cưới nhà Trai</div>
            <div class="flex justify-center my-1">
              <img
                class="rounded-md w-1/2"
                src="./assets/DSC03020.jpg"
                alt=""
              />
            </div>
            <div class="font-itim text-xl my-1 px-4">
              Sn 36 Ngõ 65 Tổ 11 Mậu Lương Kiến Hưng, Hà Đông - Hà Nội
            </div>
            <div class="flex justify-center">
              <a
                class="border-2 w-3/4 p-2 my-1 rounded-full text-blue-500 font-bold"
                href="https://maps.app.goo.gl/CvLLFHAhLPrg4Tka8"
              >
                Xem bản đồ</a
              >
            </div>
          </div>
        </div>
        <div
          class="flex justify-center md:justify-end mt-6 md:mt-0 mb-4 md:mb-0"
        >
          <div class="w-3/4 bg-white px-2 py-10 text-center p-4 border">
            <div class="font-bold text-2xl font-itim">Tiệc cưới nhà Trai</div>
            <div class="flex justify-center my-1">
              <img
                class="rounded-md w-1/2"
                src="./assets/DSC03116.jpg"
                alt=""
              />
            </div>
            <div class="font-itim text-xl my-1 px-4">
              Nhà Văn Hóa Mậu Lương, 78 P. Mậu Lương, Kiến Hưng, Hà Đông, Hà
              Nội, Việt Nam
            </div>
            <div class="flex justify-center">
              <a
                class="border-2 w-3/4 p-2 my-1 rounded-full text-blue-500 font-bold"
                href="https://maps.app.goo.gl/SXJZ4idZxgzYAa766"
              >
                Xem bản đồ</a
              >
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Album ảnh -->
    <div class="w-full border-t-4 pt-6 mt-4">
      <p class="text-center font-extrabold my-2 mb-4 text-4xl font-sans">
        Album ảnh cưới
      </p>
      <div class="w-full text-center relative flex justify-center">
        <a
          class="w-5/6"
          href="https://youtu.be/RUHelM6Qmvo?si=8oMTx7fpzm6c3x-w"
        >
          <img
            class="opacity-70"
            src="./assets/64b5ecb0289388cdd182.jpg"
            alt=""
          />
          <p
            class="text-2xl absolute top-[25%] left-[15%] md:left-[35%] md:text-6xl text-pink-500 font-black font-comic"
          >
            Xem video cưới ở đây nha
          </p>
          <p
            class="ring-1 ring-slate-200 rounded-full text-pink-500 flex justify-center absolute top-[40%] left-[45%] md:left-[50%] md:text-6xl"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="72"
              height="72"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
              class="feather feather-play"
            >
              <polygon points="5 3 19 12 5 21 5 3"></polygon>
            </svg>
          </p>
        </a>
      </div>
      <div class="grid grid-cols-2">
        <div class="col-span-1">
          <div class="p-2">
            <img
              class="rounded-lg"
              src="./assets/5b4cc0274b04eb5ab215.jpg"
              alt=""
            />
          </div>
        </div>
        <div class="col-span-1">
          <div class="p-2">
            <img
              class="rounded-lg"
              src="./assets/73b058e59cc63c9865d7.jpg"
              alt=""
            />
          </div>
        </div>
        <div class="col-span-2">
          <div class="p-2">
            <img
              class="rounded-lg"
              src="./assets/64b5ecb0289388cdd182.jpg"
              alt=""
            />
          </div>
        </div>
        <div class="col-span-1">
          <div class="p-2">
            <img class="rounded-lg" src="./assets/DSC02518.jpg" alt="" />
          </div>
        </div>
        <div class="p-2 col-span-1">
          <div class="p-2">
            <img
              class="rounded-lg"
              src="./assets/ebe3e28569a6c9f890b7.jpg"
              alt=""
            />
          </div>
        </div>
        <div class="p-2 col-span-2">
          <img
            class="rounded-lg"
            src="./assets/6b63aa212102815cd813.jpg"
            alt=""
          />
        </div>
        <div class="p-2 col-span-1">
          <div class="p-2">
            <img
              class="rounded-lg"
              src="./assets/8d571bfddfde7f8026cf.jpg"
              alt=""
            />
          </div>
          <div class="p-2">
            <img
              class="rounded-lg"
              src="./assets/b01eb2433960993ec071.jpg"
              alt=""
            />
          </div>
        </div>
        <div class="p-2 col-span-1">
          <div class="p-2">
            <img class="rounded-lg" src="./assets/DSC02690.jpg" alt="" />
          </div>
          <div class="p-2">
            <img
              class="rounded-lg"
              src="./assets/5205cb39401ae044b90b.jpg"
              alt=""
            />
          </div>
        </div>
      </div>
    </div>
    <!-- Cảm ơn -->
    <div class="w-full border-t-4 pt-6 mt-4">
      <div class="flex justify-center my-10">
        <img src="./assets/footer.jpg" alt="" />
      </div>
    </div>
  </div>
</template>

<style scoped>
.font-brush {
  font-family: "Brush Script MT", cursive;
}
.font-comic {
  font-family: "Comic Sans MS", "Comic Sans", cursive;
}
.font-itim {
  font-family: "Itim", cursive;
}
.bg-img {
  background-image: url("./assets/bg.jpg") !important;
  background-size: cover;
  background-position: center;
}
.flower {
  position: absolute;
  top: -50px; /* Bắt đầu từ trên màn hình */
  font-size: 12px;
  animation-name: fall;
  animation-timing-function: linear;
}
.img-slide {
  right: 0;
  animation-name: moveLeft;
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
@keyframes moveLeft {
  from {
    opacity: 0.4;
  }
  to {
    opacity: 1;
  }
}
</style>
