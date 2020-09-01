<template>
    <Aux>
        <Modal :show="purchasing" :modalClicked="purchaseCancelHandler">
            <OrderSummary
                :ingredients="ingredients"
                :continueHandler="purchaseContinueHandler"
                :cancelHandler="purchaseCancelHandler"
                :price="totalPrice"
            />
        </Modal>
        <Burger :ingredients="ingredients" />
        <BuildControls
            :added="addIngredienthandler"
            :removed="removeIngredientHandler"
            :disabled="disabled"
            :price="totalPrice"
            :purchasable="purchasable"
            :purchasing="purchaseHandler"
        />
    </Aux>
</template>

<script>

import Aux from "../hoc/_Aux";
import Burger from "../components/Burger/Burger";
import BuildControls from "../components/Burger/BuildControls/BuildControls";
import Modal from "../components/UI/Modal/Modal";
import OrderSummary from "../components/Burger/OrderSummary/OrderSummary";

export default {
    components: {
        Aux,
        Burger,
        BuildControls,
        Modal,
        OrderSummary,
    },
    data() {
        return {
            ingredients: {
                meat: 0,
                salad: 0,
                cheese: 0,
                bacon: 0,
            },
            INGRENDIENT_PRICES: {
                cheese: 1.5,
                salad: 0.5,
                meat: 3,
                bacon: 2,
            },
            totalPrice: 4,
            purchasable: false,
            purchasing: false,
            loading: false,
        };
    },

    methods: {
        addIngredienthandler(type) {
            const updatedIngredient = { ...this.ingredients };
            const oldCount = updatedIngredient[type];
            const updatedCount = oldCount + 1;
            updatedIngredient[type] = updatedCount;

            const ingredientPrices = { ...this.INGRENDIENT_PRICES };
            const priceAddition = ingredientPrices[type];
            const oldPrice = this.totalPrice;
            const newPrice = oldPrice + priceAddition;

            const newData = {
                ingredients: updatedIngredient,
                totalPrice: newPrice,
            };

            Object.assign(this.$data, newData);

            this.updatePurchaseState(updatedIngredient);
        },

        removeIngredientHandler(type) {
            const updatedIngredient = { ...this.ingredients };
            const oldCount = updatedIngredient[type];
            const updatedCount = oldCount - 1;
            updatedIngredient[type] = updatedCount;

            const ingredientPrices = { ...this.INGRENDIENT_PRICES };
            const priceAddition = ingredientPrices[type];
            const oldPrice = this.totalPrice;
            const newPrice = oldPrice - priceAddition;

            const newData = {
                ingredients: updatedIngredient,
                totalPrice: newPrice,
            };

            Object.assign(this.$data, newData);

            this.updatePurchaseState(updatedIngredient);
        },

        disabled(type) {
            return this.ingredients[type] <= 0;
        },

        updatePurchaseState(ingredients) {
            const sum = Object.keys(ingredients)
                .map((igKey) => {
                    return ingredients[igKey];
                })
                .reduce((sum, el) => {
                    return sum + el;
                }, 0);

            return (this.purchasable = sum > 0);
        },

        purchaseHandler() {
            this.purchasing = true;
        },

        purchaseCancelHandler() {
            this.purchasing = false;
        },

        purchaseContinueHandler() {
            this.$router.push({
                name: "Checkout",
                query: { ...this.ingredients, price: this.totalPrice },
            });
        },
    },
};
</script>

<style></style>
