<template>
    <v-dialog >
        <v-card style="width: 500px; height: 400px;">
            <v-card-text>變更卡片資料</v-card-text>
            <v-text-field v-model="tempCard.title"></v-text-field>
            <v-textarea v-model="tempCard.contents"></v-textarea>
            <v-card-actions class="justify-end">
                <v-btn color="secondary" @click="cancelDialog">Cancel</v-btn>
                <v-btn color="primary" @click="confirmDialog">Confirm</v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script setup>
    import { defineProps, defineEmits, reactive, watch } from 'vue';

    const props = defineProps({
        dialog: Boolean,
        card: Object
    });
    const emits = defineEmits(['closeDialog','updateCard']);
    
    let tempCard = reactive({title:"",contents:""});
    
    const confirmDialog = () => {
        emits('updateCard',tempCard);
        emits('closeDialog');
    };

    const cancelDialog = () => {
        emits('closeDialog');
    }

    
    watch(()=>props.dialog,()=>{
        tempCard = reactive(JSON.parse(JSON.stringify(props.card)));
    });
  
</script>