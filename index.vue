<template>
    <div>
        <div v-if="qrType=='gif'">
            <!-- <image :src="qrGif" :style="{'height': cell, 'width': cell}"></image> -->
        </div>
        <div
            v-if="qrType=='div'"
            :style="{'width': cell, 'height': cell, 'background-color': background}"
        >
            <div
                v-for="row in qrData"
                :style="{'flex-direction': 'row', 'justify-content': 'space-around', 'height': row.cell}"
            >
                <div
                    v-for="col in row.cols"
                    :style="{'flex': 1, 'height': row.cell, 'width': row.cell, 'background-color': col.background}"
                ></div>
            </div>
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
        },
        type: {
            type: String,
            default: 'div'
        },
        typeNumber: {
            type: Number,
            default: 0
        },
        errorCorrectionLevel: {
            type: String,
            default: 'H'
        }
    },
    data() {
        return {
            background: background,
            qrData: [],
            qrGif: '',
            qrType: 'div'
        }
    },
    created() {
    },
    mounted() {
        this.createQR(this.text, this.cell, this.type)
    },
    watch: {
        text(val) {
            this.createQR(val, this.cell, this.type)
        },
        cell(val) {
            this.createQR(this.text, val, this.type)
        }
    },
    methods: {
        createQR(text, cell, typ) {
            this.qrType = typ

            qrcode.stringToBytes = qrcode.stringToBytesFuncs['UTF-8']

            var qr = qrcode(this.typeNumber, this.errorCorrectionLevel)
            qr.addData(text);
            qr.make();

            if (this.qrType == 'gif') {
                this.qrGif = qr.createDataURL(this.cell, 0)
                return
            }

            this.qrType = 'div'
            this.qrData = this.createDivTag(qr, cell)
        },
        createDivTag(qr, cell) {
            // \u0020 半角空格
            var qrData = []

            var tileCell = cell / qr.getModuleCount() + 'px';

            for (var row = 0; row < qr.getModuleCount(); row++) {
                var tempRow = {
                    cell: tileCell,
                    cols: []
                }

                for (var col = 0; col < qr.getModuleCount(); col++) {
                    var tempCol = {
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
