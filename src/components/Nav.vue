<template>
    <nav>
        <router-link to="/money" class="item" active-class="selected">
            <icon name="money"/>
            记账
        </router-link>
        <router-link to="/labels" class="item" active-class="selected">
            <icon name="label"/>
            标签
        </router-link>
        <router-link to="/statistics" class="item" active-class="selected">
            <icon name="statistics"/>
            统计
        </router-link>
    </nav>
</template>

<script lang="ts">


    import Icon from '@/components/icon.vue';
    export default {
        name: 'Nav',
        components: {Icon}
    };
</script>

<style lang="scss" scoped>
    nav {
        @extend %outerShadow !optional;
        display: flex;
        flex-direction: row;
        font-size: 12px;
        > .item {
            padding: 2px 0;
            width: 33.33333%;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            .icon {
                width: 32px;
                height: 32px;
            }
        }
        > .item.selected{
            color: red;
        }
    }
</style>