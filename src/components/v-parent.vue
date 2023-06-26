<template>
    <v-container fluid>
        <v-row class="parent-background">
            <v-col
                cols="8"
                style="border-style:solid; border-width: 1px;"
            >
                <v-row>
                    <draggable
                        v-model="even"
                        group="group1"
                        @start="drag=true"
                        @end="drag=false"
                        item-key="title"
                        v-bind="dragOptions"
                        class="v-row ma-0"
                    >
                        <template #item="{element,index}">
                            <v-col cols="6">
                                <v-card
                                    :title="element.title"
                                    style="height: 169px;"
                                    class="d-flex flex-column transparent-card"
                                >
                                    <v-card-text>
                                        <v-row no-gutters>
                                            {{ element.contents }}
                                        </v-row>
                                        <v-row
                                            v-if="element.hobbys"
                                            align="center"
                                            no-gutters
                                        >
                                            興趣：
                                            <v-chip
                                                size="small"
                                            >
                                                {{ element.hobbys.name }}
                                            </v-chip>
                                        </v-row>
                                    </v-card-text>
                                    <v-card-actions class="justify-end">
                                        <v-icon
                                            :icon="mdiPencil"
                                            @click="openDialog(2*index)"
                                            color="black"
                                        />
                                        <v-icon
                                            :icon="mdiContentSave"
                                            @click="saveCard(2*index)"
                                            color="black"
                                        />
                                    </v-card-actions>
                                </v-card>
                            </v-col>
                        </template>
                    </draggable>
                </v-row>
            </v-col>

            <v-col
                cols="4"
                style="border-style:solid; border-width: 1px;"
            >
                <v-row>
                    <draggable
                        v-model="odd"
                        group="group2"
                        @start="drag=true"
                        @end="drag=false"
                        item-key="title"
                        v-bind="dragOptions"
                        class="v-row ma-0"
                    >
                        <template #item="{element,index}">
                            <v-col cols="12">
                                <v-card
                                    :title="element.title"
                                    style="height: 169px;"
                                    class="d-flex flex-column transparent-card"
                                >
                                    <v-card-text>
                                        <v-row no-gutters>
                                            {{ element.contents }}
                                        </v-row>
                                        <v-row
                                            v-if="element.hobbys?.length>0"
                                            align="center"
                                            no-gutters
                                        >
                                            興趣：
                                            <v-chip
                                                v-for="hobby in element.hobbys"
                                                :key="hobby.id"
                                                size="small"
                                            >
                                                {{ hobby.name }}
                                            </v-chip>
                                        </v-row>
                                    </v-card-text>
                                    <v-card-actions class="justify-end">
                                        <v-icon
                                            :icon="mdiPencil"
                                            @click="openDialog(1+(2*index))"
                                            color="black"
                                        />
                                        <v-icon
                                            :icon="mdiContentSave"
                                            @click="saveCard(1+(2*index))"
                                            color="black"
                                        />
                                    </v-card-actions>
                                </v-card>
                            </v-col>
                        </template>
                    </draggable>
                </v-row>
            </v-col>
        </v-row>
        <child 
            :dialog="dialog" 
            :card="propCard"
            :index="cardIndex"
            @updateCard="updateCard"
            @closeDialog="closeDialog" 
        
            v-model="dialog" 
            width="auto" 
        />
    </v-container>
</template>
  
<script setup>

    import { mdiPencil, mdiContentSave } from '@mdi/js';
    import child from '@/components/v-child.vue';

    import { ref, reactive, computed } from 'vue';
    import draggable from 'vuedraggable';
    
    const card_items = ref([
        { title: "第一張卡片", contents: "一排只能三張卡片唷！"},
        { title: "第二張卡片", contents: "ㄏㄏㄏ"},
        { title: "第三張卡片", contents: "ＸＤＤＤ"},
        { title: "第四張卡片", contents: "今天天氣真好"},
        { title: "第五張卡片", contents: "亂講 明明就很糟"},
        { title: "第六張卡片", contents: "亂講 明明就很糟"},
        { title: "第七張卡片", contents: "亂講 明明就很糟"},
        { title: "第八張卡片", contents: "亂講 明明就很糟"},
        { title: "第九張卡片", contents: "亂講 明明就很糟"},
    ]);


    const even = computed({
        get: () => card_items.value.filter((item,index) => index%2==0 ),
        set: (val) => {
            for(let i = 0 ; i < val.length;i++){
                card_items.value[i*2] = val[i];
            }
        }
    });

    const odd = computed({
        get: () => card_items.value.filter((item,index) => index%2!=0 ),
        set: (val) => {
            for(let i = 0 ; i < val.length;i++){
                card_items.value[1+(i*2)] = val[i];
            }
        }
    });

    const dialog = ref(false);
    
    const cardIndex = ref(0);

    let propCard = reactive({title:"",contents:""});

    const openDialog = (index) => {
        cardIndex.value = index;
        propCard = reactive(card_items.value[index]);
        dialog.value = true;
    };

    const updateCard = (newCard) => {
        card_items.value[cardIndex.value] = {...newCard};
    };

    const closeDialog = () => {
        dialog.value = false;
    };

    const saveCard = (index) => {
        console.log(`您儲存的是第 ${index+1} 張卡，卡片資訊為：title: ${card_items.value[index].title}, contents: ${card_items.value[index].contents}, hobbys: ${card_items.value[index].hobbys}`);
    };

    const dragOptions = computed(() => {
        return {
            animation: 200,
            group: "description",
            disabled: false,
            ghostClass: "ghost"
        };
    });


</script>

<style>
    .parent-background {
    background-image: url('/src/assets/background.jpeg');
    background-size: cover;
    background-position: center;
    }

    .transparent-card{
    background-color: rgba(255, 255, 255, 0.5);
    }

    .ghost {
    opacity: 0.5;
    background: #c8ebfb;
    }

</style>