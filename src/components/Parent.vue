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
        :itemIndex="itemIndex" 
        :itemTitle = "itemTitle" 
        :itemContents="itemContents"
        @closeDialog="closeDialog" 
        @updateTitle="updateTitle"
        @updateContents="updateContents"
        v-model="dialog" 
        width="auto" 
        />
    </v-container>
</template>
  
<script setup>
    import { defineProps, defineEmits } from 'vue';
    import { ref } from 'vue';
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
    
    const itemIndex = ref(0);
    const itemTitle = ref("");
    const itemContents = ref("");

    const emits = defineEmits(['closeDialog','updateTitle','updateContents']);

    const openDialog = (index) => {
        itemIndex.value = index;
        itemTitle.value = card_items[index].title;
        itemContents.value = card_items[index].contents;
        dialog.value = true;
    };
    const closeDialog = () => {
        dialog.value = false;
    };
    const updateTitle = (index,newTitle) => {
        card_items[index].title = newTitle;
    }
    const updateContents = (index,newContents) => {
        card_items[index].contents = newContents;
    }

    const saveCard = (index) => {
        console.log(`您儲存的是第 ${index+1} 張卡，卡片資訊為：title: ${card_items[index].title}, contents: ${card_items[index].contents}`);
    }


</script>
  