<template>

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
</template>

<script setup>


const props=defineProps({
  showorder:Boolean,
  FinalOrder:Object
})
const emits=defineEmits(['show-order']);

const showOrder=()=>{
  emits('show-order',FinalOrder)
}


</script>