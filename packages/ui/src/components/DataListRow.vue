<template>
    <div class="SharpDataList__row px-3" :class="classes">
        <div class="row gx-0">
            <div class="col d-flex flex-column justify-content-center position-relative">
                <div class="SharpDataList__cols py-3">
                    <div class="row gx-n2 gx-md-n3">
                        <template v-for="(column, i) in columns">
                            <div :class="[
                                header ? 'SharpDataList__th' : 'SharpDataList__td',
                                colClasses(column)
                            ]">
                                <slot name="cell" :row="row" :column="column">
                                    <template v-if="column.html">
                                        <div v-html="row[column.key]" class="SharpDataList__td-html-container"></div>
                                    </template>
                                    <template v-else>
                                        {{ row[column.key] }}
                                    </template>
                                </slot>
                            </div>
                        </template>
                    </div>
                </div>
                <template v-if="hasLink">
                    <a class="SharpDataList__row-link position-absolute inset-0" :href="url"></a>
                </template>
            </div>
            <div class="col-sm-auto align-self-center">
                <template v-if="$scopedSlots.append">
                    <div class="SharpDataList__row-append pb-3 pt-sm-3">
                        <slot name="append" v-bind="this" />
                    </div>
                </template>
                <template v-else>
                    <div class="SharpDataList__row-spacer"></div>
                </template>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            columns: Array,
            row: {
                type: Object,
                default: ()=>({})
            },
            url: String,
            header: Boolean,
            highlight: Boolean,
        },
        data() {
            return {
                isHighlighted: this.highlight,
            }
        },
        watch: {
            highlight() {
                this.isHighlighted = this.highlight;
            }
        },
        computed: {
            hasLink() {
                return !!this.url;
            },
            classes() {
                return {
                    'SharpDataList__row--header': this.header,
                    'SharpDataList__row--disabled': !this.header && !this.hasLink,
                    'SharpDataList__row--highlight': this.isHighlighted,
                }
            },
        },
        methods: {
            colClasses(column) {
                const { size, sizeXS, hideOnXS } = column;
                return {
                    'col': sizeXS === 'fill' && !hideOnXS,
                    [`col-${sizeXS}`]: sizeXS !== 'fill' && !hideOnXS,
                    'col-md': size === 'fill',
                    [`col-md-${size}`]: size !== 'fill',
                    'd-none d-md-block': hideOnXS,
                };
            },
            toggleHighlight(highlight) {
                this.isHighlighted = highlight;
            }
        }
    }
</script>
