<template>
    <Layout>
        <Tabs class-prefix="type" :data-source="typeList" :value.sync="type"/>
        <div>

        </div>
        <div class="notes-wrapper">
            <ol>
                <li v-for="(group,index) in groupedList" :key="index">
                    <h3 class="title">{{beautify(group.title)}}</h3>
                    <ol>
                        <li v-for="item in group.items" :key="item.id"
                            class="record">
                            <span>{{tagString(item.tags)}}</span>
                            <span class="notes">111{{item.notes}}</span>
                            <span>￥{{item.amount}} </span>
                        </li>
                    </ol>
                </li>
            </ol>
        </div>
    </Layout>

</template>


<script lang="ts">
    import Vue from 'vue';
    import {Component} from 'vue-property-decorator';
    import Tabs from '@/components/Tabs.vue';
    import intervalList from '@/constants/intervalList';
    import typeList from '@/constants/typeList';
    import dayjs from 'dayjs';
    import clone from '@/lib/clone';

    @Component({
        components: {Tabs}
    })
    export default class Statistics extends Vue {
        beautify(string: string) {
            const day = dayjs(string);
            const now = dayjs();
            if (dayjs(string).isSame(now, 'day')) {
                return '今天';
            } else if ((dayjs(string).isSame(now.subtract(1, 'day'), 'day'))) {
                return '昨天';
            } else if ((dayjs(string).isSame(now.subtract(2, 'day'), 'day'))) {
                return '前天';
            } else if (day.isSame(now, 'year')) {
                return day.format('M月D日');
            } else {
                return day.format('YYYY年MM月DD日');
            }
        }

        tagString(tags: Tag[]) {
            return tags.length === 0 ? '无' : tags.join(',');
        }


        get recordList() {
            return (this.$store.state as RootState).recordList;
        }

        get groupedList() {
            const {recordList} = this;
            if (recordList.length === 0) {
                return [];
            }
            const newList = clone(recordList)
                .filter(r => r.type === this.type)
                .sort((a, b) => dayjs(b.createAt).valueOf() - dayjs(a.createAt).valueOf());
            type Result = { title: string; total?: number; items: RecordItem[] }[]
            const result: Result = [{title: dayjs(newList[0].createAt).format('YYYY-MM-DD'), items: [newList[0]]}];
            for (let i = 1; i < newList.length; i++) {
                const current = newList[i];
                const last = result[result.length - 1];
                if (dayjs(last.title).isSame(dayjs(current.createAt), 'day')) {
                    last.items.push(current);
                } else {
                    result.push({title: dayjs(current.createAt).format('YYYY-MM-DD'), items: [current]});
                }
            }
            return result;
        }


        beforeCreate() {
            this.$store.commit('fetchRecords');
        }

        interval = 'day';
        type = '-';
        intervalList = intervalList;
        typeList = typeList;
    }
</script>

<style scoped lang="scss">
    %item {
        padding: 8px 16px;
        line-height: 24px;
        display: flex;
        justify-content: space-between;
        align-content: center;
    }

    .title {
        @extend %item;
    }

    .record {
        background: white;
        @extend %item;
    }

    .notes {
        margin-right: auto;
        margin-left: 16px;
        color: #999;
    }

    ::v-deep .type-tabs-item {
        background: #c4c4c4;

        &.selected {
            background: white;

            &::after {
                display: none;
            }
        }
    }

    ::v-deep .interval-tabs-item {
        height: 48px;
    }

    ::v-deep .notes-wrapper {
        overflow: scroll;
        height: 90%;
    }
</style>