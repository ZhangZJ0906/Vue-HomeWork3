<template>
  <div id="root">
    <div class="container mt-5">
      <div class="row">
        <!-- 菜單 -->
        <div class="col-md-4">
          <div class="list-group" v-for="item in data" :key="item.id" @click.prevent="addItem(item)">
            <a href="#" class="list-group-item list-group-item-action">
              <div class="d-flex w-100 justify-content-between">
                <h5 class="mb-1">{{ item.name }}</h5>
                <small>{{ item.price }}</small>
              </div>
              <p class="mb-1">{{ item.description }}</p>
            </a>
          </div>
        </div>
        <!--新增刪除訂單 -->
        <div class="col-md-8">
          <table class="table">
            <thead>
              <tr>
                <th scope="col" width="50">操作</th>
                <th scope="col">品項</th>
                <th scope="col">描述</th>
                <th scope="col" width="90">數量</th>
                <th scope="col">單價</th>
                <th scope="col">小計</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(orderItem, index) in order" :key="orderItem.id">
                <td><button type="button" class="btn btn-sm" @click.prevent="removeItem(index)">x</button></td>
                <td>{{ orderItem.name }}</td>
                <td><small>{{ orderItem.description }}</small></td>
                <td>
                  <select class="form-select" v-model="orderItem.quantity">
                    <option v-for="i in maxQuantity" :key="i" :value="i">{{ i }}</option>
                  </select>
                </td>
                <td>{{ orderItem.price }}</td>
                <td>{{ orderItem.price * orderItem.quantity }}</td>
              </tr>
            </tbody>
          </table>
          <div class="text-end mb-3">
            <h5>總計: <span>{{ total }}</span></h5>
          </div>
          <textarea class="form-control mb-3" rows="3" placeholder="備註" v-model="note"></textarea>
          <div class="text-end">
            <button class="btn btn-primary" @click.prevent="submitOrder">送出</button>
          </div>
        </div>
      </div>
      <hr />
      <!-- 訂單送出 -->
      <div class="row justify-content-center" v-if="showorder">
        <div class="col-8">
          <div class="card">
            <div class="card-body">
              <div class="card-title">
                <h5>訂單</h5>
                <table class="table">
                  <thead>
                    <tr>
                      <th scope="col">品項</th>
                      <th scope="col">數量</th>
                      <th scope="col">小計</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="item in FinalOrder.items" :key="item.id">
                      <td>{{ item.name }}</td>
                      <td>{{ item.quantity }}</td>
                      <td>{{ item.price * item.quantity }}</td>
                    </tr>
                  </tbody>
                </table>
                <div class="text-end">備註: <span>{{ FinalOrder.note }}</span></div>
                <div class="text-end">
                  <h5>總計: <span>{{ FinalOrder.total }}</span></h5>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
const showorder = ref(false);
const order = ref([]);
const orders = ref([]);
const maxQuantity = 50;
const note = ref('');
const FinalOrder = ref(null);


const data = [
  {
    "id": 1,
    "name": "珍珠奶茶",
    "description": "香濃奶茶搭配QQ珍珠",
    "price": 50
  },
  {
    "id": 2,
    "name": "冬瓜檸檬",
    "description": "清新冬瓜配上新鮮檸檬",
    "price": 45
  },
  {
    "id": 3,
    "name": "翡翠檸檬",
    "description": "綠茶與檸檬的完美結合",
    "price": 55
  },
  {
    "id": 4,
    "name": "四季春茶",
    "description": "香醇四季春茶，回甘無比",
    "price": 45
  },
  {
    "id": 5,
    "name": "阿薩姆奶茶",
    "description": "阿薩姆紅茶搭配香醇鮮奶",
    "price": 50
  },
  {
    "id": 6,
    "name": "檸檬冰茶",
    "description": "檸檬與冰茶的清新組合",
    "price": 45
  },
  {
    "id": 7,
    "name": "芒果綠茶",
    "description": "芒果與綠茶的獨特風味",
    "price": 55
  },
  {
    "id": 8,
    "name": "抹茶拿鐵",
    "description": "抹茶與鮮奶的絕配",
    "price": 60
  }
];

const addItem = (item) => {
  const itemIndex = order.value.find((order) => order.id === item.id);
  if (itemIndex) {
    itemIndex.quantity += 1;

  } else {
    order.value.push({ ...item, quantity: 1 });
  }
};
const removeItem = (index) => {
  order.value.splice(index, 1);
};
const submitOrder = () => {
  if (order.value.length > 0) {
    const temporder = {
      items: [...order.value],
      note: note.value,
      total: total.value
    }
    orders.value.push(temporder);
    FinalOrder.value = orders.value[orders.value.length - 1];
  }
  showorder.value = true
  order.value = [];
  note.value = '';
}




const total = computed(() => {
  return order.value.reduce((sum, order) => sum + order.price * order.quantity, 0)
});

</script>