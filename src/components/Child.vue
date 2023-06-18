<template>
    <v-dialog >
        <v-card style="width: 500px; height: 400px;">
            <v-card-text>088le</v-card-text>
            <v-text-field v-model="tempTitle"></v-text-field>

            <v-textarea v-model="tempContents"></v-textarea>

            <v-card-actions class="justify-end">
                <v-btn color="secondary" @click="cancelDialog">Cancel</v-btn>
                <v-btn color="primary" @click="confirmDialog">Confirm</v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script setup>
    import { defineProps, defineEmits, ref, watch } from 'vue';

    const props = defineProps({
        dialog: Boolean,
        itemIndex: Number,
        itemTitle: String,
        itemContents: String
    });
    const emits = defineEmits(['closeDialog','updateTitle','updateContents']);
    
    
    const tempTitle = ref("");
    const tempContents = ref("");

    
    const confirmDialog = () => {
        emits('updateTitle',props.itemIndex,tempTitle.value);
        emits('updateContents',props.itemIndex,tempContents.value);
        emits('closeDialog');
    };

    const cancelDialog = () => {
        emits('closeDialog');
    }

    
    watch(()=>props.dialog,(newValue)=>{
        if(newValue == true){
            tempTitle.value = props.itemTitle;
            tempContents.value = props.itemContents;
        }
        
    });
  
</script>