<template>
    <ion-page>
        <ion-content>
            <p>Index Page</p>
            <ion-button @click="checkNfcStatus">Check NFC</ion-button>
            <p>{{ status }}</p>
        </ion-content>
    </ion-page>
</template>
<script setup>
import { NFC } from 'capacitor-nfc';
const status = ref('')
const nfc = new NFC();
function checkNfcStatus () {
    nfc.checkNfcEnabled().then(enabled => {
        if (enabled) {
            // NFC is enabled
            status.value = "enabled"
            nfc.requestNfcPermission().then(permission => {
                if (permission === 'granted') {
                    // NFC permission granted
                    status.value = status.value + ', permission granted'
                    nfc.addNfcTagDiscoveredListener().subscribe(tag => {
                        // Tag discovered, handle it
                        console.log(tag);
                    });
                }
            });
        }
    });
}
</script>
