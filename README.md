# weex-qrcode
qrcode component for weex, written by vuejs

Example:

```
<template>
    <weex-qrcode :text="qrText" :cell="qrCell"></weex-qrcode>
</template>

<script>
import WeexQrcode from 'weex-qrcode'

export default {
    components: {
      WeexQrcode
    },
    data() {
        return {
            qrText: 'https://www.google.com',
            qrCell: 256
        }
    }
    ...
    ...
}
</script>
```