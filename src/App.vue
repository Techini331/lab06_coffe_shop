<template>
  <div>
    <h1>จองร้านกาแฟ</h1>
    <div class="coffee-shops">
      <div v-for="(shop, index) in coffeeShops" :key="index" class="coffee-shop">
        <h2>{{ shop.name }}</h2>
        <img :src="shop.image" :alt="shop.name" />
        <button @click="reserveTable(shop)">จองโต๊ะ</button>
      </div>
    </div>

    <h1>รายการจอง</h1>
    <div v-if="reservations.length === 0">
      <p>ยังไม่มีรายการจอง.</p>
    </div>
    <div v-else>
      <table>
        <thead>
          <tr>
            <th>ร้าน</th>
            <th>ชื่อ</th>
            <th>โทรศัพท์</th>
            <th>วันที่</th>
            <th>เวลา</th>
            <th>จำนวนโต๊ะ</th>
            <th>ยกเลิกการจอง</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(reservation, index) in reservations" :key="index">
            <td>{{ reservation.shop.name }}</td>
            <td>{{ reservation.name }}</td>
            <td>{{ reservation.phone }}</td>
            <td>{{ reservation.date }}</td>
            <td>{{ reservation.time }}</td>
            <td>{{ reservation.numTables }}</td>
            <td><button class="cancel-button" @click="cancelReservation(index)">ยกเลิก</button></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      coffeeShops: [
        {
          name: 'Smoothcup Coffee',
          image: 'https://image.makewebeasy.net/makeweb/m_1920x0/sEBH5Q7g0/SHOP2/WebHug2_1.jpg',
        },
        {
          name: 'Chill Coffee',
          image: 'https://i.pinimg.com/564x/77/0a/1e/770a1e29215dc0a3398c3c8cf882efb1.jpg',
        },
        {
          name: 'Yensabai Coffee',
          image: 'https://i.pinimg.com/564x/22/f5/1e/22f51e1e04194c4ea668b9ec05241452.jpg',
        },
        {
          name: 'Lamoon Coffee',
          image: 'https://i.pinimg.com/564x/58/e9/e7/58e9e710cff1fbd332ff6c85c2df4f7d.jpg',
        },
        {
          name: 'Lucky Coffee',
          image: 'https://i.pinimg.com/564x/6e/c7/c3/6ec7c3c92d2b1fce462cfbdb9ceaab92.jpg',
        },
        {
          name: 'Goodmood Coffee',
          image: 'https://i.pinimg.com/564x/a5/33/81/a533814d24c7c9a5c6727429b8780eee.jpg',
        },
      ],
      reservations: [],
      reservationData: {
        name: '',
        phone: '',
        date: '',
        time: '',
        numTables: 1,
        shop: null,
        imageLink: '', // เพิ่มฟิลด์นี้
      },
    };
  },
  methods: {
    reserveTable(shop) {
      this.reservationData.shop = shop;
      this.reservationData.imageLink = shop.image; // กำหนดลิงค์รูปจากร้านที่ถูกเลือก
      // เพิ่มโค้ดส่วนการจองโต๊ะที่นี่
      // คุณสามารถใช้ this.reservationData ในการเข้าถึงข้อมูลการจอง
      // และเพิ่มข้อมูลการจองลงใน this.reservations
      // และล้างข้อมูลการจองใน this.reservationData เมื่อจองสำเร็จ
    },
    cancelReservation(index) {
      // เพิ่มโค้ดส่วนการยกเลิกการจองที่นี่
      // โดยใช้ index เพื่อระบุการจองที่จะยกเลิก
      this.reservations.splice(index, 1); // ยกเลิกการจองโดยลบออกจาก this.reservations
    },
  },
};
</script>

<style scoped>

.coffee-shop {
  width: calc(33.33% - 20px);
  margin-bottom: 20px;
  border: 1px solid #ccc;
  padding: 10px;
  text-align: center;
  box-sizing: border-box;
  display: flex; /* เพิ่มการใช้ Flexbox เพื่อจัดกล่องรูปภาพและปุ่ม */
  flex-direction: column; /* จัดเรียงแนวตั้ง */
}

img {
  max-width: 100%;
  height: auto;
  object-fit: cover; /* ทำให้รูปภาพเต็มกรอบโดยไม่บิดเบือนรูปภาพ */
  flex: 1; /* ทำให้รูปภาพขยายตามความสูงของพื้นที่ใน div.coffee-shop */
}

img {
  max-width: 100%;
  height: auto;
}
.coffee-shops {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
}

.coffee-shop {
  border: 1px solid #ccc;
  padding: 10px;
  margin: 10px;
  text-align: center;
}

img {
  max-width: 100%;
  height: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  padding: 10px;
  border: 1px solid #ccc;
  text-align: center;
}

thead {
  background-color: #f0f0f0;
}

tbody tr:nth-child(even) {
  background-color: #f2f2f2;
}

  tbody tr:hover {
  background-color: #e0e0e0;
}

.cancel-button {
  background-color: #f44336;
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
  border-radius: 5px;
}

.cancel-button:hover {
  background-color: #d32f2f;
}
</style>