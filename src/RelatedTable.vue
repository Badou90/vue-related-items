<template>
    <div>
        <table class="table table-bordered table-striped display dataTable related-table">
            <thead>
                <tr>
                    <th v-for="header in columns">{{ header }}</th>
                    <th></th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="item in itemsLocal">
                    <td v-for="(translation, column) in columns"> {{ item[column] }} </td>
                    <td class="text-right"> <a href="#" @click.prevent="removeRelatedItem(item)" class="btn btn-danger"><i class="fa fa-remove"></i></a> </td>
                </tr>
                <tr>
                    <td v-for="(translation, column) in columns"><input class="form-control" type="text" :placeholder="translation" v-model="newItem[column]"></td>
                    <td class="text-right"> <a href="#" @click.prevent="addRelatedItem" class="btn btn-success"><i class="fa fa-check"></i></a> </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    import _ from 'lodash';

    export default {
        props: [
            'items',
            'columns',
            'createUrl',
            'deleteUrl',
            'entryId',
        ],

        data() {
            return {
                itemsLocal: this.items,
                newItem: {},
            };
        },

        methods: {
            addRelatedItem() {
                axios.post(this.createUrl, this.newItem).then((response) => {
                    this.newItem.id = response.data.id;
                }).then(() => {
                    this.itemsLocal.push(Object.assign({}, this.newItem));
                    this.newItem = {};   
                });
            },

            removeRelatedItem(removed) {
                this.itemsLocal.splice(_.findIndex(this.itemsLocal, function(item) {
                    return item === removed;
                }), 1);

                axios.post(this.deleteUrl, {id: removed.id});
            },
        },
    }
</script>

<style lang="scss">
    .related-table {
        th {
            text-transform: capitalize;
        }
    }
</style>