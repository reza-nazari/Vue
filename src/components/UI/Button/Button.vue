<template>
    <button v-on:click="clicked" class="Button" :class="btnType">
        <slot></slot>
    </button>
</template>

<script>
export default {
    props: ["btnType", "clicked"],
};
</script>

<style>
.Button {
    background-color: transparent;
    border: none;
    color: white;
    outline: none;
    cursor: pointer;
    font: inherit;
    padding: 10px;
    margin: 10px;
    font-weight: bold;
}

.Button:first-of-type {
    margin-left: 0;
    padding-left: 0;
}

.Success {
    color: #5c9210;
}

.Danger {
    color: #944317;
}
</style>
