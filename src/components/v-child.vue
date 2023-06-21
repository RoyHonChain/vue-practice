<template>
    <v-dialog>
        <v-card style="width: 500px; height: auto;">
            <v-card-text>變更卡片資料</v-card-text>
            <v-text-field v-model="tempCard.title" />
            <v-textarea v-model="tempCard.contents" />            
            <v-select
                label="Select"
                :multiple="!(props.index%2==0)"
                :chips="!(props.index%2==0)"
                :items="all_hobbies"
                item-title="name"
                return-object
                clearable
                v-model="tempCard.hobbys"
            />
            <v-card-actions class="justify-end">
                <v-btn
                    color="secondary"
                    @click="cancelDialog"
                >
                    Cancel
                </v-btn>
                <v-btn
                    color="primary"
                    @click="confirmDialog"
                >
                    Confirm
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script setup>
    import { defineProps, defineEmits, reactive, watch } from 'vue';

    const all_hobbies = reactive([]);
    Object.assign(all_hobbies, [
        { id: "772723", value: "tennis", name: "打網球" },
        { id: "747381", value: "piano", name: "彈鋼琴" },
        { id: "587319", value: "game", name: "玩遊戲" },
        { id: "297634", value: "hiking", name: "爬山" },
    ]);
    
    const props = defineProps({
        dialog: Boolean,
        // eslint-disable-next-line vue/require-default-prop
        card: Object,
        index: Number
    });
    const emits = defineEmits(['closeDialog','updateCard']);
    
    let tempCard = reactive({title:"",contents:"",hobbys:""});
    
    const confirmDialog = () => {
        emits('updateCard',tempCard);
        emits('closeDialog');
        console.log(props.index);
    };

    const cancelDialog = () => {
        emits('closeDialog');
    };

    
    watch(()=>props.dialog,()=>{
        tempCard = reactive(JSON.parse(JSON.stringify(props.card)));
    });
  
</script>