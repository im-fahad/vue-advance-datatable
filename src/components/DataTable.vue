<template>
    <div class="v-datatable" :id="id">
        <div class="v-datatable__header" v-if="header">
            <div class="v-datatable__header-slot">
                <slot name="header"/>
            </div>
            <div class="v-datatable__header-search">
                <input type="text" class="v-datatable__header-search-input" @change="searchData">
                <button class="v-datatable__header-search-button">
                    <img src="../assets/search_dark.png" alt="">
                </button>
            </div>
        </div>
        <div class="v-datatable__inner">
            <div class="v-datatable__placeholder mt-2" v-if="!items.length || loading">
                <div v-if="loading" class="main-loader main-loader--20vh">
                    <!--                    <loader :type="loaderType"/>-->
                    loading...
                </div>
                <slot v-if="!loading && $slots['placeholder']" name="placeholder"/>
                <span v-if="!loading && !$slots['placeholder']">No data found!</span>
            </div>

            <table v-if="items.length && !loading" class="main-table" :class="'text-'+textAlign">
                <thead v-if="headers.length" class="main-table__header">
                <tr class="main-table__header-tr">
                    <th class="main-table__header-th" v-for="(item, i) in headers" v-text="item.text" :key="i"
                        @click="sort(item)"
                        :class="setClass(item.sortable, 'sortable') + ' ' + isActiveTh(item.sortKey) + ' ' + setClass(item.textAlign, 'text-'+item.textAlign)"/>
                </tr>
                </thead>
                <tbody class="main-table__body">
                <slot/>
                </tbody>
            </table>
        </div>
        <div class="v-datatable__footer" v-if="pagination && items.length > 0">
            <div class="v-datatable__footer-item v-datatable__footer-item--perpage">
                Per Page:
                <!--                <g-dropdown :caret="true">-->
                <!--                    <template v-slot:button>-->
                <!--                        {{ perPage }}-->
                <!--                    </template>-->
                <!--                    <g-dropdown-item-button v-for="(item, i) in perPageItems" :key="i"-->
                <!--                                            :class="perPage === item ? 'active':''" :title="item"-->
                <!--                                            @onPress="updatePerPage(item)"/>-->
                <!--                </g-dropdown>-->
            </div>
            <div class="v-datatable__footer-item v-datatable__footer-item--info"
                 v-if="paginationTotal > 0">
                Showing
                <strong>{{ getShowingItemsLength }}</strong>
                Of
                <strong>{{ paginationTotal }}</strong>
            </div>
            <div class="v-datatable__footer-item v-datatable__footer-item--pagination">
                <ul class="v-datatable-pagination">
                    <li class="v-datatable-pagination__item">Prev</li>
                    <li class="v-datatable-pagination__item">1</li>
                    <li class="v-datatable-pagination__item">2</li>
                    <li class="v-datatable-pagination__item">3</li>
                    <li class="v-datatable-pagination__item">4</li>
                    <li class="v-datatable-pagination__item">5</li>
                    <li class="v-datatable-pagination__item">Next</li>
                </ul>
                <pagination :limit="1" :data="pagination"
                            @pagination-change-page="paginationChangePage"></pagination>
            </div>
        </div>
    </div>
</template>

<script>
import pagination from 'laravel-vue-pagination'

export default {
    name: 'v-datatable',
    components: {
        pagination
    },
    props: {
        id: {
            type: String,
            require: false,
            default: 'dataTable' + Number(Math.random() * 100).toFixed(0)
        },
        header: {
            type: Boolean,
            require: false,
            default: true
        },
        headers: {
            type: Array,
            require: true,
            default: () => {
                return []
            }
        },
        items: {
            type: Array,
            require: true,
            default: () => {
                return []
            }
        },
        perPageItems: {
            type: Array,
            require: true,
            default: () => {
                return [10, 20, 30, 40, 50]
            }
        },
        pagination: {
            type: Object,
            require: true,
            default: () => {
                return {}
            }
        },
        textAlign: {
            type: String,
            require: false,
            default: 'left'
        },
        rowsPerPage: {
            type: Number,
            require: false,
            default: 10
        },
        loading: {
            type: Boolean,
            require: false,
            default: false
        },
        loaderType: {
            type: String,
            require: false,
            default: 'dotted'
        },
    },
    data() {
        return {
            selected: {
                key: '',
                sorted: false
            },
            sortBy: '',
            perPage: this.rowsPerPage,
        }
    },
    computed: {
        paginationTotal() {
            if (!this.pagination) return 0;
            if (this.pagination.meta) {
                return this.pagination.meta.total
            }
            if (this.pagination.total) {
                return this.pagination.total
            }
            return 0
        },
        formattedPaginationData() {
            if (!this.pagination) return 0;
            if (this.pagination.meta) {
                return this.pagination.meta
            }
            return this.pagination
        },
        getShowingItemsLength() {
            if (!this.formattedPaginationData) return 0;
            if (this.formattedPaginationData.last_page === this.formattedPaginationData.current_page) {
                return this.formattedPaginationData.total
            }
            return Number(this.items.length) * Number(this.formattedPaginationData.current_page)
        },
        copyData() {
            return JSON.parse(JSON.stringify(this.items))
        }
    },
    methods: {
        sort(item) {
            if (!item.sortable) return;

            if (this.selected.key !== item.sortKey) {
                this.selected.sorted = false;
            }
            this.selected.key = item.sortKey;
            this.selected.sorted = !this.selected.sorted;
            this.items = this.getSortedData(this.items, item.sortKey);
            this.$emit('sorted', this.items);
        },

        getSortedData(items, key) {
            let keys = key.split('.')
            return items.sort((a, b) => {
                keys.forEach(key => {
                    if (a[key] == null || b[key] == null) return;
                    // eslint-disable-next-line no-prototype-builtins
                    if (!a.hasOwnProperty(key) || !b.hasOwnProperty(key)) return;
                    a = a[key];
                    b = b[key];
                })

                if (typeof a == 'number' && typeof b == 'number') {
                    if (!this.selected.sorted) {
                        return a - b
                    }
                    return b - a
                }

                if (typeof a == 'string' && typeof b == 'string') {
                    if (a.toUpperCase() > b.toUpperCase() && !this.selected.sorted) {
                        return 1
                    }
                    if (a.toUpperCase() < b.toUpperCase() && this.selected.sorted) {
                        return 1
                    }
                    return -1
                }

            })
        },
        isActiveTh(key) {
            if (this.selected.key !== key) return '';

            if (this.selected.sorted) {
                return 'up'
            }
            return 'down'
        },
        paginationChangePage(page) {
            this.selected.key = '';
            this.selected.sorted = false;
            this.$emit('pagination-change-page', page)
        },
        updatePerPage(value) {
            this.perPage = value;
            this.selected.key = '';
            this.selected.sorted = false;
            this.$emit('per-page', value)
        },
        setClass(item, className) {
            if (item) {
                return className
            }
            return ''
        },

        searchData(event) {
            console.log('copy', this.copyData)
            let newData = this.copyData.filter(item => item[this.sortBy].toLowerCase().includes(event.target.value.toLowerCase()))
            this.$emit('search', newData)
        }
    },
};
</script>

<style lang="scss">
@import "../assets/scss/variables";
@import "../assets/scss/mixins";
@import "../assets/scss/datatable";
</style>