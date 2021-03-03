<template>
    <router-link :to="/users/ + product.id" class="item">
        <img :src="product.img" :alt="product.title">
        <p>
            {{
                product.title
            }}
        </p>
    </router-link>
</template>

<script>
export default {
    props: {
        product: {
            type: Object,
            required: true
        }
    }
}
</script>

<style lang="scss">
    .item__wrapper {
        display: flex;
        align-items: center;
        justify-content: space-between;
        flex-wrap: wrap;
    }
    .item {
        max-width: 48%;
        text-align: center;
        cursor: pointer;
        p {
            font-size: 22px;
        }
    }
</style>