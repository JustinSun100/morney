<template>
    <Layout class="wrapper">
        <div class="tags">
            <router-link class="tag" v-for="tag in tags" :key="tag.id"
                         :to="`/labels/edit/${tag.id}`">
                <span>{{tag.name}}</span>

                <icon name="right"/>
            </router-link>
        </div>
        <div class="createTag-wrapper">
            <Button class="createTag" @click="createTag">新增标签</Button>
        </div>
    </Layout>
</template>

<script lang="ts">

    import Vue from 'vue';
    import {Component} from 'vue-property-decorator';
    import Button from '@/components/Button.vue';


    @Component({
        components: {Button}
    })
    export default class Labels extends Vue {
        get tags(){
            return this.$store.state.tagList;
        }
        beforeCreate() {
            this.$store.commit('fetchTags');
        }

        createTag() {
            const name = window.prompt('请输入新的标签名');
            if (name === '') {
                alert('标签名不能为空');
            } else if (name === null) {
                return;
            } else {
                this.$store.commit('createTag', name);
            }
        }
    }
</script>
<style lang="scss" scoped>

    .tags {
        overflow: auto;
        height: 80%;
        background: white;
        font-size: 16px;
        padding-left: 16px;

        > .tag {
            min-height: 44px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            border-bottom: 1px solid #e6e6e6;

            svg {
                margin-right: 18px;
            }
        }
    }

    .createTag {
        border: none;
        color: white;
        background: #767676;
        border-radius: 4px;
        padding: 0 16px;
        height: 40px;

        &-wrapper {
            height: 15%;
            text-align: center;
            padding: 16px;
            margin-top: 44-16px;
        }
    }
</style>
