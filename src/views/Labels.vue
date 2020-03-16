<template>
    <Layout>
        <ol class="tags">
            <li v-for="tag in tags" :key="tag">
                <span>{{tag}}</span>

                <icon name="right"/>
            </li>
        </ol>
        <div class="createTag-wrapper">
            <button class="createTag" @click="createTag">新增标签</button>
        </div>
    </Layout>
</template>

<script lang="ts">

    import Vue from 'vue';
    import {Component} from 'vue-property-decorator';
    import tagListModel from '@/models/tagListModel';


    @Component
    export default class Labels extends Vue {
        tags = tagListModel.fetch();

        createTag() {
            const name = window.prompt('请输入标签名');
            if (name) {
                const message = tagListModel.create(name);
                if (message === 'duplicated') {
                    window.alert('标签名重复了');
                } else if (message === 'success') {
                    window.alert('添加成功');
                }
            }
        }
    }
</script>
<style lang="scss" scoped>
    .tags {
        background: white;
        font-size: 16px;
        padding-left: 16px;

        > li {
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
            text-align: center;
            padding: 16px;
            margin-top: 44-16px;
        }
    }
</style>
