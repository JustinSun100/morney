<template>
    <div class="tags">
        <div class="new">
            <button @click="create">新增标签</button>
        </div>
        <ul class="current">
            <li v-for="tag in tagList" :key="tag.id"
                :class="{selected:selectTags.indexOf(tag)>=0}"
                @click="toggle(tag)"
            >{{tag.name}}
            </li>
        </ul>
    </div>

</template>

<script lang="ts">
    import Vue from 'vue';
    import {Component} from 'vue-property-decorator';


    @Component
    export default class Tags extends Vue {
        get tagList(){
            return this.$store.state.tagList
        }

        selectTags: string[] = [];

        created(){
            this.$store.commit('fetchTags')
        }

        toggle(tag: string) {
            const index = this.selectTags.indexOf(tag);
            if (index >= 0) {
                this.selectTags.splice(index, 1);
            } else {
                this.selectTags.push(tag);
            }
            this.$emit('update:value', this.selectTags);
        }

        create() {
            const name = window.prompt('请输入新的标签名');
            if (name === '') {
                alert('标签名不能为空');
            } else if (name === null) {
                return;
            } else {
               this.$store.commit('createTag',name)
            }

        }
    }
</script>

<style lang="scss" scoped>
    .tags {
        font-size: 14px;
        padding: 16px;
        flex-grow: 1;
        display: flex;
        flex-direction: column-reverse;
        background: white;

        > .current {
            display: flex;
            flex-wrap: wrap;

            > li {
                $bg: #d9d9d9;
                background: $bg;
                $h: 24px;
                height: $h;
                line-height: $h;
                border-radius: $h/2;
                padding: 0 16px;
                margin-right: 12px;
                margin-top: 4px;

                &.selected {
                    background: darken($bg, 50%);
                    color: #f5f5f5;
                }
            }
        }

        > .new {
            padding-top: 16px;

            button {
                background: transparent;
                border: none;
                color: #999;
                border-bottom: 1px solid;
                padding: 0 4px;
            }
        }
    }

</style>