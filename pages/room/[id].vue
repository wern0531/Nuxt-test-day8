<script setup>
import axios from "axios";
const router = useRouter();
const route = useRoute();
const roomData = ref("");
const getRoomData = async () => {
  const req = await axios(
    `https://nuxr3.zeabur.app/api/v1/rooms/${route.params.id}`
  );
  roomData.value = req.data.result;
  console.log(roomData.value);
};
await getRoomData();
// console.log(req);

// 串接 API 取得房型詳細資料
// API path : https://nuxr3.zeabur.app/api/v1/rooms/{id}
// 將資料渲染至下方的 div.room-page 區塊
</script>

<template>
  <h2>房型詳細頁面</h2>

  <div class="container">
    <button @click="router.go(-1)">回上一頁</button>
    <div class="row justify-content-center">
      <div class="col-md-6">
        <div class="room-page">
          <div class="room-header">
            <h1 class="room-name">{{ roomData.name }}</h1>
            <p class="room-description">
              {{ roomData.description }}
            </p>
          </div>

          <div class="room-gallery">
            <img
              :src="roomData.imageUrl"
              alt="尊爵雙人房主圖"
              class="room-main-image"
            />
            <div class="room-image-list">
              <div v-for="imageUrlList in roomData.imageUrlList">
                <img :src="imageUrlList" />
              </div>
            </div>
          </div>

          <div class="room-info">
            <div class="info-block">
              <h2>房間資訊</h2>
              <p>面積: {{ roomData.areaInfo }}</p>
              <p>床型: {{ roomData.bedInfo }}</p>
              <p>最多容納人數: {{ roomData.maxPeople }}</p>
              <p>價格: NT${{ roomData.price }}</p>
            </div>

            <div class="info-block">
              <h2>房間配置</h2>
              <ul v-for="layoutInfo in roomData.layoutInfo">
                <li v-if="layoutInfo.isProvide">
                  {{ layoutInfo.title }}: 提供
                </li>
                <li v-else>{{ layoutInfo.title }}: 不提供</li>
              </ul>
            </div>

            <div class="info-block">
              <h2>房內設施</h2>
              <ul v-for="facilityInfo in roomData.facilityInfo">
                <li v-if="facilityInfo.isProvide">
                  {{ facilityInfo.title }}: 提供
                </li>
                <li v-else>{{ facilityInfo.title }}: 不提供</li>
              </ul>
            </div>

            <div class="info-block">
              <h2>客房備品</h2>
              <ul v-for="amenityInfo in roomData.amenityInfo">
                <li v-if="amenityInfo.isProvide">
                  {{ amenityInfo.title }}: 提供
                </li>
                <li v-else>{{ amenityInfo.title }}: 不提供</li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.room-page {
  max-width: 1200px;
  margin: 0 auto;
  background-color: #fff;
  padding: 20px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
}

.room-header {
  text-align: center;
  margin-bottom: 30px;
}

.room-name {
  font-size: 2rem;
  color: #333;
}

.room-description {
  font-size: 1rem;
  color: #666;
}

.room-gallery {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 30px;
}

.room-main-image {
  width: 100%;
  max-width: 600px;
  height: auto;
  border-radius: 10px;
  margin-bottom: 20px;
}

.room-image-list {
  display: flex;
  justify-content: center;
  gap: 10px;
}

.room-image-list img {
  width: 100px;
  height: auto;
  border-radius: 5px;
  cursor: pointer;
  transition: transform 0.3s;
}

.room-image-list img:hover {
  transform: scale(1.1);
}

.room-info {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.info-block {
  background-color: #f0f0f0;
  padding: 15px;
  border-radius: 10px;
}

.info-block h2 {
  font-size: 1.5rem;
  margin-bottom: 10px;
  color: #444;
}

.info-block p,
.info-block ul {
  font-size: 1rem;
  color: #555;
}

.info-block ul {
  list-style: none;
  padding-left: 0;
}

.info-block ul li {
  margin-bottom: 5px;
}
</style>
