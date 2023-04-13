<template>
    <div class="q-pa-md q-gutter-sm" style="max-width: 450px;">
        <q-dialog v-model="loadingAct">
            <q-card style="max-width: 450px;">
                <q-card-section class="text-white q-py-md shadow-2">
                    Lorem ipsum dolor sit amet consectetur adipisicing elit. Quia iusto porro, quod exercitationem magni
                    impedit
                    fugiat quibusdam eum, id ipsam dolor labore, laboriosam quisquam autem. Dignissimos molestiae doloremque
                    asperiores! Eius! Lorem ipsum dolor, sit amet consectetur adipisicing elit. At et itaque deserunt
                    corporis.
                    Impedit voluptas quos debitis quidem temporibus autem nisi mollitia minus repellat laborum eius, nobis
                    eveniet
                    tempora architecto.
                </q-card-section>
                <q-inner-loading :showing="visible" label="Por favor aguarde..." label-class="text-dark"
                    label-style="font-size: 1.1em" size="100px" color="red" />
            </q-card>
        </q-dialog>
        <!-- <q-dialog v-model="deleteConfirmationAct" persistent>
            <q-card>
                <q-card-section class="row items-center">
                    <q-avatar icon="signal_wifi_off" color="primary" text-color="white" />
                    <span class="q-ml-sm">You are currently not connected to any network.</span>
                </q-card-section>

                <q-card-actions align="right">
                    <q-btn flat label="Cancel" color="primary" v-close-popup />
                    <q-btn flat label="Turn on Wifi" color="primary" v-close-popup />
                </q-card-actions>
            </q-card>
        </q-dialog> -->
    </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from 'vue'

export default defineComponent({
    name: 'MessagesComponent',
    props: {
        loading: {
            type: Boolean,
            default: false
        },
        deleteConfirmation: {
            type: Boolean,
            default: false
        }
    },
    setup (props) {
        const visible = ref(true)
        const loadingAct = ref(false)
        const deleteConfirmationAct = ref(false)
        watch(() => ([props.loading, props.deleteConfirmation]), (first, second) => {
            (props.loading) ? loadingAct.value = true : loadingAct.value = false
            // (props.deleteConfirmation) ? loadingAct.value = true : loadingAct.value = false
            console.log(
                "Watch props.selected function called with args:",
                first,
                second, 'act->', props.loading
            )
        })
        // console.log('loading->', props.dialogData)
        return {
            visible, loadingAct, deleteConfirmationAct

        }
    }
})
</script>
