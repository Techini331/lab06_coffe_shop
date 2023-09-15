<template>
  <div>
    <!-- เพิ่มรูปภาพพื้นหลัง -->
    <div class="background-image">
      <!-- ใช้ลิงค์ของรูปภาพพื้นหลังที่คุณต้องการ -->
      <img
        src="https://i.pinimg.com/564x/ac/6e/e0/ac6ee077a578c10b71b546b74d62c261.jpg"
        alt="Background Image"
      />
    </div>
    <h1>จองร้านกาแฟ☕</h1>
    <br>
    <div class="coffee-shops">
      <div v-for="(shop, index) in coffeeShops" :key="index" class="coffee-shop">
        <h2>{{ shop.name }}</h2>
        <img :src="shop.image" :alt="shop.name" />

        <!-- เพิ่มปุ่ม "จองโต๊ะ" และเรียกใช้ฟังก์ชัน reserveTable -->
        <button @click="reserveTable(shop)">จองโต๊ะ</button>

        <!-- แบบฟอร์มการจองโต๊ะ -->
        <div v-if="shop === selectedShop">
          <h2>แบบฟอร์มการจองโต๊ะ</h2>
          <form @submit.prevent="submitReservation">
            <div class="form-group">
              <label for="shopSelect">ร้าน</label>
              <span>{{ selectedShop.name }}</span>
            </div>
            <br />
            <div class="form-group">
              <label for="customerName">ชื่อ</label>
              <input type="text" id="customerName" v-model="reservationData.name" required />
            </div>
            <br />
            <div class="form-group">
              <label for="customerPhone"> เบอร์โทรศัพท์</label>
              <input type="tel" id="customerPhone" v-model="reservationData.phone" required />
            </div>
            <br />
            <div class="form-group">
              <label for="reservationDate">วันที่</label>
              <input type="date" id="reservationDate" v-model="reservationData.date" required />
            </div>
            <br />
            <div class="form-group">
              <label for="reservationTime">เวลา</label>
              <input type="time" id="reservationTime" v-model="reservationData.time" required />
            </div>
            <br />
            <div class="form-group">
           <label for="tableNumber">จำนวนโต๊ะ</label>
          <input type="number" id="tableNumber" v-model="reservationData.tableNumber" required />
          </div>
        <br>
            <button type="submit">ยืนยันการจอง</button>
            <button type="button" @click="cancelReservation">ยกเลิกการจอง</button>
          </form>
        </div>
      </div>
    </div>
    <br /><br /><br />
    <!-- ตารางการจอง -->
<div v-if="reservations.length > 0">
  <h2>ตารางการจองโต๊ะ</h2>
  <br />
  <table>
    <thead>
      <tr>
        <th>ลำดับคิว</th>
        <th>ชื่อร้าน</th>
        <th>ชื่อ (ผู้จองโต๊ะ)</th>
        <th>เบอร์โทรศัพท์ (ผู้จองโต๊ะ)</th>
        <th>วันที่</th>
        <th>เวลา</th>
        <th>จำนวนโต๊ะ</th> <!-- เพิ่มคอลัมน์ใหม่ -->
        <th>ที่อยู่ร้าน</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(reservation, index) in reservations" :key="index">
        <td>{{ index + 1 }}</td>
        <td>{{ reservation.shop }}</td>
        <td>{{ reservation.name }}</td>
        <td>{{ reservation.phone }}</td>
        <td>{{ reservation.date }}</td>
        <td>{{ reservation.time }}</td>
        <td>{{ reservation.tableNumber }}</td> <!-- แสดงจำนวนโต๊ะที่จอง -->
        <td>{{ getCoffeeShopAddress(reservation.shop) }}</td>
        
      </tr>
    </tbody>
  </table>
</div>

    </div>
  <br /><br />
</template>

<script>
export default {
  data() {
    return {
      coffeeShops: [
        {
          name: 'Smoothcup Coffee',
          image: 'https://image.makewebeasy.net/makeweb/m_1920x0/sEBH5Q7g0/SHOP2/WebHug2_1.jpg',
          address: '123 ถนนงามตา,กรุงเทพมหานคร',
          
        },
        {
          name: 'Chill Coffee',
          image: 'https://i.pinimg.com/564x/77/0a/1e/770a1e29215dc0a3398c3c8cf882efb1.jpg',
          address: '456 ถนนสุขสันต์,กรุงเทพมหานคร',
        },
        {
          name: 'Yensabai Coffee',
          image: 'https://i.pinimg.com/564x/22/f5/1e/22f51e1e04194c4ea668b9ec05241452.jpg',
          address: '876 ถนนข้าวเหนียว,กรุงเทพมหานคร',
        },

        {
          name: 'Lamoon Coffee',
          image: 'https://i.pinimg.com/564x/58/e9/e7/58e9e710cff1fbd332ff6c85c2df4f7d.jpg',
          address: '915 ถนนข้าวจ้าว,กรุงเทพมหานคร',
        },

        {
          name: 'Lucky Coffee',
          image: 'https://i.pinimg.com/564x/6e/c7/c3/6ec7c3c92d2b1fce462cfbdb9ceaab92.jpg',
          address: '558 ถนนวันดี,กรุงเทพมหานคร',
        },

        {
          name: 'Goodmood Coffee',
          image: 'https://i.pinimg.com/564x/a5/33/81/a533814d24c7c9a5c6727429b8780eee.jpg',
          address: '168 ถนนทองหล่อ,กรุงเทพมหานคร',
        },
       
      ],
      selectedShop: null, // เพิ่มฟิลด์เก็บร้านที่เลือก
      reservationData: {
        name: '',
        phone: '',
        date: '',
        time: '',
        tableNumber: null, // เพิ่มฟิลด์เก็บเลขที่โต๊ะที่จอง
      },
      reservations: [], // รายการการจอง
    };
  },
  methods: {
    reserveTable(shop) {
      // เมื่อคลิกปุ่ม "จองโต๊ะ" ให้กำหนดร้านที่เลือกและแสดงแบบฟอร์มการจอง
      this.selectedShop = shop;
    },
   
    submitReservation() {
      // เมื่อยืนยันการจอง ให้เพิ่มข้อมูลการจองลงในรายการ
      this.reservations.push({
        shop: this.selectedShop.name,
        name: this.reservationData.name,
        phone: this.reservationData.phone,
        date: this.reservationData.date,
        time: this.reservationData.time,
        tableNumber: this.reservationData.tableNumber, // เพิ่มเลขที่โต๊ะที่จอง
  });
      // รีเซ็ตข้อมูลการจองและร้านที่เลือก
      this.reservationData.name = '';
      this.reservationData.phone = '';
      this.reservationData.date = '';
      this.reservationData.time = '';
      this.reservationData.tableNumber = null; // รีเซ็ตเลขที่โต
      this.reservationData.tableNumber = null; // รีเซ็ตเลขที่โต๊ะ
      this.selectedShop = null; // ปิดแบบฟอร์มการจอง
    },
    cancelReservation() {
      // เมื่อคลิกปุ่ม "ยกเลิกการจอง" ให้ล้างข้อมูลการจองและปิดแบบฟอร์มการจอง
      this.reservationData.name = '';
      this.reservationData.phone = '';
      this.reservationData.date = '';
      this.reservationData.time = '';
      this.reservationData.tableNumber = null; // รีเซ็ตเลขที่โต๊ะ
      this.selectedShop = null;
    },
    getCoffeeShopAddress(shopName) {
      const coffeeShop = this.coffeeShops.find(shop => shop.name === shopName);
      return coffeeShop ? coffeeShop.address : '';
    },
  },
};
</script>

<style scoped>
button {
        margin-right: 1px; /* ระยะห่างด้านขวาของปุ่ม */
    }
    
.coffee-shop {
  width: calc(26.33% - 20px);
  margin-bottom: 20px;
  border: 1px solid #080707;
  padding: 10px;
  text-align: center;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}

img {
  max-width: 100%;
  height: auto;
  object-fit: cover;
  flex: 1;
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
  border: 1px solid #000000;
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
  background-color: #f5f5f5; /* สีพื้นหลังสำหรับตาราง */
}

th, td {
  padding: 10px;
  border: 1px solid #ffffff;
  text-align: center;
  background-color: rgb(0, 0, 0); /* สีพื้นหลังของเซลล์ตาราง */
}

thead {
  background-color: #000000;
  color: white; /* สีข้อความสำหรับส่วนหัวของตาราง */
}

tbody tr:nth-child(even) {
  background-color: #e0e0e0; /* สีพื้นหลังสำหรับแถวคู่ในตาราง */
}

tbody tr:hover {
  background-color: #1d1b1b; /* สีพื้นหลังเมื่อโฮเวอร์แถวในตาราง */
}

.background-image {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1; /* ให้รูปอยู่ด้านหลังของเนื้อหา */
}

.background-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  opacity: 0.5; /* ปรับความโปร่งแสงของรูปพื้นหลังตามต้องการ */
}


</style>