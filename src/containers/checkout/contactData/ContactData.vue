<template>
    <div class="ContactData">
        <h4>Enter your Contact Data</h4>
        <form v-if="!loading">
            <input
                class="Input"
                type="text"
                name="name"
                placeholder="Your name"
                v-model="name"
            />
            <input
                class="Input"
                type="email"
                name="email"
                placeholder="Your mail"
                v-model="email"
            />
            <input
                class="Input"
                type="text"
                name="street"
                placeholder="Your Street"
                v-model="address.street"
            />
            <input
                class="Input"
                type="text"
                name="postal"
                placeholder="Postal Code"
                v-model="address.postalCode"
            />
            <Button btnType="Success" :clicked="orderHandler">
                Order
            </Button>
        </form>
        <Spinner v-else />
    </div>
</template>

<script>
import Button from "../../../components/UI/Button/Button";
import Spinner from "../../../components/UI/Spinner/Spinner";
import axios from "../../../axios-orders";
export default {
    components: {
        Button,
        Spinner,
    },
    props: ["ingredients", "totalPrice"],
    data() {
        return {
            name: "",
            email: "",
            address: {
                street: "",
                postalCode: "",
            },
            loading: false,
        };
    },
    methods: {
        orderHandler() {
            event.preventDefault();

            this.loading = true;
            const order = {
                ingredients: this.ingredients,
                price: this.totalPrice,
                customer: {
                    name: "reza",
                    address: {
                        street: "TestStreet1",
                        zipCode: "4531",
                        country: "Iran",
                    },
                    email: "test@test.com",
                    deliveryMethods: "fastest",
                },
            };
            console.log(order);
            axios
                .post("/order.json", order)
                .then((response) => {
                    this.loading = false;
                    this.$router.push("/");
                })
                .catch((err) => {
                    this.loading = false;
                });
        },
    },
};
</script>

<style>
.ContactData {
    margin: 20px auto;
    width: 80%;
    text-align: center;
    box-shadow: 0 2px 3px #ccc;
    border: 1px solid #eee;
    padding: 10px;
    box-sizing: border-box;
}

.Input {
    display: block;
}

@media (min-width: 600px) {
    .ContactData {
        width: 500px;
    }
}
</style>
