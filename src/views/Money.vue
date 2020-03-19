<template>
    <Layout class-prefix="layout">
        {{record}}
        <NumberPad @update:value="onUpdateAmount" @submit="saveRecord"/>
        <Types :value.sync="record.type"/>
        <div class="notes">
            <FormItem filename="备注" placeholder="这里输入备注" @update:value="onUpdateNotes"/>
        </div>
        <Tags/>
    </Layout>
</template>

<script lang="ts">
    import Vue from 'vue';
    import NumberPad from '@/components/Money/NumberPad.vue';
    import Tags from '@/components/Money/Tags.vue';
    import Types from '@/components/Money/Types.vue';
    import FormItem from '@/components/Money/FormItem.vue';
    import {Component, Watch} from 'vue-property-decorator';


    @Component({
            components: {Tags, FormItem, Types, NumberPad}
        }
    )
    export default class Money extends Vue {
        get recordList() {
            return this.$store.state.recordList;
        }

        tags = [];//store.tagList;
        record: RecordItem = {
            tags: [], notes: '', type: '-', amount: 0
        };

        created() {
            this.$store.commit('fetchRecords');
        }

        onUpdateNotes(value: string) {
            this.record.notes = value;
        }

        onUpdateAmount(value: number) {
            this.record.amount = value;
        }

        saveRecord() {
            this.$store.commit('createRecord', this.record);
        }

    }
</script>

<style lang="scss">
    .layout-content {
        display: flex;
        flex-direction: column-reverse;
    }
</style>
<style lang="scss" scoped>
    @import "~@/assets/styles/helper.scss";

    .notes {
        padding: 12px 0;
    }

</style>
