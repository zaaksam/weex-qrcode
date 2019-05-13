<template>
    <div :style="{'width': cell, 'height': cell, 'background-color': background}">
        <div v-for="row in qrData" :style="{'flex-direction': 'row', 'height': row.cell}">
            <div
                v-for="col in row.cols"
                :style="{'flex': 1, 'height': row.cell, 'width': row.cell, 'background-color': col.background}"
            ></div>
        </div>
    </div>
</template>

<script>
import qrcode from 'qrcode-generator'

const background = '#ffffff'
const foreground = '#000000'

export default {
    props: {
        text: {
            type: String,
            default: 'weex-qrcode'
        },
        cell: {
            type: Number,
            default: 100
        }
    },
    data() {
        return {
            background: background,
            qrData: []
        }
    },
    created() {
    },
    mounted() {
        this.qrData = this.build(this.text, this.cell)
    },
    watch: {
        text(val) {
            this.qrData = this.build(val, this.cell)
        },
        cell(val) {
            this.qrData = this.build(this.text, val)
        }
    },
    methods: {
        build(text, cell) {
            let qrData = []

            let qr = qrcode(0, 'H')
            qr.addData(text);
            qr.make();

            let tileCell = cell / qr.getModuleCount();

            for (let row = 0; row < qr.getModuleCount(); row++) {
                let tempRow = {
                    cell: tileCell,
                    cols: []
                }

                for (let col = 0; col < qr.getModuleCount(); col++) {
                    let tempCol = {
                        background: background
                    }

                    if (qr.isDark(row, col)) {
                        tempCol.background = foreground
                    }

                    tempRow.cols.push(tempCol)
                }

                qrData.push(tempRow)
            }

            return qrData
        }
    },
    computed: {
    }
}
</script>