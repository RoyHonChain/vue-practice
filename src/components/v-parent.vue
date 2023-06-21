<template>
    <v-container fluid>
        <v-row>
            <v-col
                cols="8"
                style="border-style:solid; border-width: 1px;"
            >
                <v-row>
                    <v-col
                        v-for="(card, index) in even(card_items)"
                        :key="2*index"
                        cols="6"
                    >
                        <v-card
                            :title="card.title"
                            :text="card.contents"
                        >
                            <v-row
                                class="justify-end"
                                style="margin:5px;"
                            >
                                <v-icon
                                    :icon="mdiPencil"
                                    @click="openDialog(2*index)"
                                    color="grey"
                                />
                                <v-icon
                                    :icon="mdiContentSave"
                                    @click="saveCard(2*index)"
                                    color="grey"
                                />
                            </v-row>
                        </v-card>
                    </v-col>
                </v-row>
            </v-col>

            <v-col
                cols="4"
                style="border-style:solid; border-width: 1px;"
            >
                <v-row>
                    <v-col
                        v-for="(card, index) in odd(card_items)"
                        :key="1+(2*index)"
                        cols="12"
                    >
                        <v-card
                            :title="card.title"
                            :text="card.contents"
                        >
                            <v-row
                                class="justify-end"
                                style="margin:5px;"
                            >
                                <v-icon
                                    :icon="mdiPencil"
                                    @click="openDialog(1+(2*index))"
                                    color="grey"
                                />
                                <v-icon
                                    :icon="mdiContentSave"
                                    @click="saveCard(1+(2*index))"
                                    color="grey"
                                />
                            </v-row>
                        </v-card>
                    </v-col>
                </v-row>
            </v-col>
        </v-row>
        <child 
            :dialog="dialog" 
            :card="propCard"
            @updateCard="updateCard"
            @closeDialog="closeDialog" 
        
            v-model="dialog" 
            width="auto" 
        />
    </v-container>
</template>
  
<script setup>
    import { ref, reactive } from 'vue';
    import { mdiPencil, mdiContentSave } from '@mdi/js';
    import child from '@/components/v-child.vue';

    const card_items = [
        { title: "第一張卡片", contents: "一排只能三張卡片唷！" },
        { title: "第二張卡片", contents: "ㄏㄏㄏ" },
        { title: "第三張卡片", contents: "ＸＤＤＤ" },
        { title: "第四張卡片", contents: "今天天氣真好" },
        { title: "第五張卡片", contents: "亂講 明明就很糟" },
        { title: "第六張卡片", contents: "亂講 明明就很糟" },
        { title: "第七張卡片", contents: "亂講 明明就很糟" },
        { title: "第八張卡片", contents: "亂講 明明就很糟" },
        { title: "第九張卡片", contents: "亂講 明明就很糟" },
    ];
    const even = (card_items) => card_items.filter((item,index) => index%2==0 );
    const odd = (card_items) => card_items.filter((item,index) => index%2!=0 );

    const dialog = ref(false);
    
    const cardIndex = ref(0);

    let propCard = reactive({title:"",contents:""});


    const openDialog = (index) => {
        cardIndex.value = index;
        propCard = reactive(card_items[index]);
        dialog.value = true;
    };
    const closeDialog = () => {
        dialog.value = false;
    };

    const updateCard = (newCard) => {
        card_items[cardIndex.value] = {...newCard};
    };

    const saveCard = (index) => {
        console.log(`您儲存的是第 ${index+1} 張卡，卡片資訊為：title: ${card_items[index].title}, contents: ${card_items[index].contents}`);
    };


</script>
  