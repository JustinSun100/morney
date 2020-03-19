<template>
    <layout>
        <div class="navBar">
            <icon class="leftIcon" name="left" @click.native="goback"/>
            <span>编辑标签</span>
            <span class="rightIcon"></span>
        </div>
        <div class="form-wrapper">
            <FormItem :value="currentTag.name"
                      @update:value="update"
                      filename="标签名" placeholder="请输入标签名"/>
        </div>
        <div class="button-wrapper">
            <Button @click="remove">删除标签</Button>
        </div>
    </layout>
</template>

<script lang="ts">
    import Vue from 'vue';
    import {Component} from 'vue-property-decorator';
    import FormItem from '@/components/Money/FormItem.vue';
    import Button from '@/components/Button.vue';

    @Component({
        components: {FormItem, Button},

    })
    export default class EditLabel extends Vue {
        get currentTag(){
            return this.$store.state.currentTag
        }

        created() {
            const id=this.$route.params.id
            this.$store.commit('fetchTags')
            this.$store.commit('setCurrentTag',id)
            if (!this.currentTag) {
                this.$router.replace('/404');
            }
        }

        update(name: string) {
            if (this.currentTag) {
                this.$store.commit('updateTag',{id:this.currentTag.id,name})
            }
        }

        remove() {
            if (this.currentTag) {
                this.$store.commit('removeTag',this.currentTag.id)
            }
        }

        goback() {
            this.$router.back();
        }

    }
</script>

<style lang="scss" scoped>
    .navBar {
        text-align: center;
        font-size: 16px;
        background: white;
        padding: 12px 0;
        display: flex;
        justify-content: space-between;
        align-items: center;

        > .leftIcon {
            width: 24px;
            height: 24px;
        }

        > .rightIcon {
            width: 24px;
            height: 24px;
        }
    }

    .form-wrapper {
        background: white;
        margin-top: 8px;
    }

    .button-wrapper {
        margin-top: 44-16px;
        padding: 16px;
        text-align: center;
    }

</style>