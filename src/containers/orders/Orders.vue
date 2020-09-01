<template>
    <div>
        <Order
            v-for="order in orders"
            :key="order.id"
            :ingredients="order.ingredients"
            :price="order.price"
        />

      
    </div>
</template>

<script>
import axios from "../../axios-orders";
import Order from "../../components/Order/Order";

export default {
    components: {
        Order,
    },
    data() {
        return {
            loading: false,
            orders: [],
        };
    },
    beforeMount() {
        alert('if you are in iran,use proxy!!!')
        axios
            .get("/order.json")
            .then((res) => {
                const fetchOrders = [];
                for (let key in res.data) {
                    fetchOrders.push({
                        ...res.data[key],
                        id: key,
                    });
                }
                this.loading = false;
                this.orders = fetchOrders;
            })
            .catch((err) => (console.log(err)));
    },
};
</script>

<style></style>
