<template>
    <div class="table">
        <ag-grid-vue
            style="height: 100%"
            class="ag-theme-alpine"
            :columnDefs="columnDefs"
            :rowData="rowData"
            :defaultColDef="defaultColDef"
            :groupIncludeTotalFooter="true"
            @row-selected="onRowSelected"
            :rowSelection="'multiple'"
        ></ag-grid-vue>
    </div>
</template>

<script>
import { AgGridVue } from "ag-grid-vue";
import "ag-grid-enterprise";
import "ag-grid-community/dist/styles/ag-grid.css";
import "ag-grid-community/dist/styles/ag-theme-alpine.css";

export default {
    name: "test-table",
    data() {
        return {
            val8: ["string1", "string2", "string3", "string4", "string5"],
            rowData: [],
            columnDefs: [
                {
                    headerName: "GROUP1",
                    children: [
                        {
                            headerName: "col1",
                            field: "val6",
                        },
                        {
                            headerName: "col2",
                            field: "val1",
                        },
                        {
                            headerName: "col3",
                            field: "val2",
                            aggFunc: (items) =>
                                items.values
                                    .reduce((sum, cur) => sum + cur, 0)
                                    .toFixed(2),

                            valueFormatter: `value +" " + "кг"`,
                        },
                    ],
                },
                {
                    headerName: "GROUP2",
                    children: [
                        {
                            headerName: "col4",
                            field: "val4",
                            valueGetter: this.sumGetter,
                            aggFunc: "sum",
                        },
                        {
                            headerName: "col5",
                            field: "val7",
                            cellStyle: {
                                "font-weight": "bold",
                                "text-decoration": "underline",
                            },
                        },
                        {
                            headerName: "col6",
                            field: "val8",
                            filter: "agSetColumnFilter",
                        },
                    ],
                },
            ],

            defaultColDef: {
                flex: 1,
                floatingFilter: true,
            },
        };
    },

    methods: {
        createData() {
            for (let i = 0; i < 100; i++) {
                const dataItem = {
                    val1: Math.random().toString(36).substring(1, 11),
                    val2: +(Math.random() * 100).toFixed(2),
                    val3: +(Math.random() * 100).toFixed(4),
                    val4: Math.round(Math.random() * 100),
                    val5: Math.round(Math.random() * 100),
                    val6: `/${Math.random().toString(36).substring(1, 11)}`,
                    val7: Math.random().toString(36).substring(1, 11),
                    val8: this.val8[
                        Math.floor(Math.random() * this.val8.length)
                    ],
                };

                this.rowData.push(dataItem);
            }
        },

        sumGetter(param) {
            return param.data.val4 + param.data.val5;
        },

        onRowSelected(event) {
            window.alert(JSON.stringify(event.data));
        },
    },
    beforeMount() {
        this.createData();
        console.log(this.rowData);
    },

    components: {
        AgGridVue,
    },
};
</script>

<style>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}
.table {
    height: 100vh;
    width: 100vw;
    padding: 10px;
}
</style>
