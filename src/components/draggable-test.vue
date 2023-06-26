<template>
    <div>
        <draggable
            v-model="even2"
            group="group1"
            @start="drag=true"
            @end="drag=false"
            item-key="id"
        >
            <template #item="{element,index}">
                <div>
                    <div>{{ index }}</div>
                    <div>{{ element.name }}</div>
                </div>
            </template>
        </draggable>
    </div>
</template>
  
<script setup>
    import draggable from 'vuedraggable';
    import {defineComponent, ref, computed} from 'vue';
  
    defineComponent({
        components: [{'draggable': draggable},]
    });
  
    const list = ref(
        [
            {id: 1, name: 'Item 1'},
            {id: 2, name: 'Item 2'},
            {id: 3, name: 'Item 3'},
            {id: 4, name: 'Item 4'},
            {id: 5, name: 'Item 5'},
            {id: 6, name: 'Item 6'},
            {id: 7, name: 'Item 7'},
            {id: 8, name: 'Item 8'},
        ]
    );

    const even = list.value.filter((item,index) => index%2==0 );
    const even2 = computed({
        get: () => list.value.filter((item,index) => index%2==0 ),
        set: (val) => {
            for(let i = 0 ; i < val.length;i++){
                list.value[i*2] = val[i];
            }
            console.log(list.value);
        }
    });
</script>