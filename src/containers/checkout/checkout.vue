<template>
    <div>
        <CheckoutSummary
            :ingredients.sync="ingredients"
            :checkoutCancelled="checkoutCancelledHandler"
            :checkoutContinued="checkoutContinuedHandler"
        />

        <router-view
            :ingredients="ingredients"
            :totalPrice="price"
        ></router-view>
    </div>
</template>

<script>
import CheckoutSummary from "../../components/Order/CheckoutSummary/CheckoutSummary";
export default {
    components: {
        CheckoutSummary,
    },
    data() {
        return {
            ingredients: null,
            price: 0,
            tt: 0,
        };
    },

    beforeMount() {
        const query = this.$route.query;
        const ingredients = {};
        let price = 0;
        for (let param in query) {
            if (param === "price") {
                price = query["price"];
            } else {
                ingredients[param] = JSON.parse(query[param]);
            }
        }
        this.ingredients = ingredients;
        this.price = price;
    },
    methods: {
        checkoutCancelledHandler() {
            this.$router.push("/");
        },

        checkoutContinuedHandler() {
            const path = `/checkout/contact-data`;
            if (this.$route.path !== path)
                this.$router.push({
                    name: "contact-data",
                });
        },
    },
};
</script>

<style></style>
