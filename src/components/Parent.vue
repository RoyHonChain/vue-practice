<template>
    <v-container fluid>
        <v-row>
            <v-col v-for="(card, index) in card_items" :key="index" cols="4">
                <v-card :title = card.title :text = card.contents >
                    <v-row class="justify-end" style="margin:5px;">
                        <v-icon :icon="mdiPencil" @click="openDialog(index)" color="grey"/>
                        <v-icon :icon="mdiContentSave" @click="saveCard(index)" color="grey"/>
                    </v-row>
                </v-card>
            </v-col>
        </v-row>
        <Child 
        :dialog="dialog" 
        :card="card"
        @updateCard="updateCard"
        @closeDialog="closeDialog" 
        
        v-model="dialog" 
        width="auto" 
        />
    </v-container>
</template>
  
<script setup>
    import { defineEmits } from 'vue';
    import { ref, reactive } from 'vue';
    import { mdiPencil, mdiContentSave } from '@mdi/js'
    import Child from '@/components/Child.vue'

    const card_items = [
        { title: "第一張卡片", contents: "一排只能三張卡片唷！" },
        { title: "第二張卡片", contents: "ㄏㄏㄏ" },
        { title: "第三張卡片", contents: "XDDD" },
        { title: "第四張卡片", contents: "今天天氣真好" },
        { title: "第五張卡片", contents: "亂講 明明就很糟" }
    ];

    const dialog = ref(false);
    
    const cardIndex = ref(0);

    let card = reactive({title:"",contents:""});

    const emits = defineEmits(['closeDialog','updateCard']);

    const openDialog = (index) => {
        cardIndex.value = index;
        card = reactive(card_items[index]);
        dialog.value = true;
    };
    const closeDialog = () => {
        dialog.value = false;
    };

    const updateCard = (newCard) => {
        card_items[cardIndex.value] = {...newCard};
    }

    const saveCard = (index) => {
        console.log(`您儲存的是第 ${index+1} 張卡，卡片資訊為：title: ${card_items[index].title}, contents: ${card_items[index].contents}`);
    }


</script>
  