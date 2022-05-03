<template>
    <div class="w-max">
        <div class="main-container w-[250px] h-[40px] bg-white relative">
            <span @click="openClose" class="main-container-after"><i id="arrow-toggle" class="fa-solid fa-angle-up"></i></span>
            <input type="text" name="input" class="absolute" id="input-element" placeholder="Select">
        </div>
        <div id="list" class="scaleY-0-item visibility-hidden">
            <ul id="item-list">
                <li v-for="(data) in data" @click="handleList" class="list-item" :data-index="data['id']" :data-value="data[searchValue]">{{data[searchValue]}}</li>
            </ul>
        </div>
    </div>
    <select id="input-req" multiple :name="name" :value="modelValue" @change="$emit('update:modelValue', $event.target.value)">
        <option v-for="data in data" :value="data['id']">{{data[searchValue]}}</option>
    </select>
</template>

<script>
import {ref} from "vue";

export default {
    name: "MultipleSelect",
    props: ['modelValue', 'data', 'searchValue', 'name'],
    setup(props) {
        let dataArray = ref([])
        let test = ref([])
        const openClose = () => {
            let component = document.getElementById('arrow-toggle')
            let list = document.getElementById('list')
            if (component.classList.contains('fa-angle-up')) {
                component.classList.remove('fa-angle-up')
                component.classList.add('fa-angle-down')
                list.classList.toggle('scaleY-0-item')
                list.classList.toggle('visibility-hidden')

            } else {
                component.classList.add('fa-angle-up')
                component.classList.remove('fa-angle-down')
                list.classList.toggle('scaleY-0-item')
                list.classList.toggle('visibility-hidden')
            }
        }
        const handleList = (e) => {
            let select = document.getElementById('input-req')
            let event = new Event('change')
            for (let index in select.options) {
                if(e.target.dataset['index'] === select.options[index].value) {
                    select.options[index].selected = !select.options[index].selected;
                }
            }
            select.dispatchEvent(event)
        }
        return {
            handleList, openClose, dataArray, test
        }
    }
}
</script>

<style scoped lang="scss">

#input-element {
    border: 0;
    outline: 0;
    width: calc(250px - (40px + 1%));
    height: calc(40px - 10%);
    padding: .5rem;
    :focus {
        outline: none !important;
    }
}

.scaleY-0-item {
    transform: scaleY(0);
}

.scaleY-full-item {
    transform-origin: top;
    transform: scaleY(1);
}

.visibility-hidden {
    max-height: 0;
}

#list {
    max-height: 100px;
    overflow-y: auto;
    overflow-x: hidden !important;
    transform-origin: top;
    transition: all 250ms ease-in-out;
}

#item-list {
    background-color: white;
    border-left: 1px solid black;
    border-right: 1px solid black;
    border-bottom: 1px solid black;
    width: 250px;
    :last-child {
        border-bottom: none;
    }
}

.list-item {
    padding: .25rem;
    border-bottom: 1px solid black;
    transition: all 250ms ease-in-out;
    cursor: pointer;
    width: inherit;
    background-color: transparent;
    &-active {
        background-color: #5cd74d;
    }
    &:hover {
        background-color: #5cd74d;
    }
}

.main-container {
    border: 1px solid black;
}

.main-container-after {
    position: absolute;
    top: 0;
    right: 0;
    height: inherit;
    width: 40px;
    background-color: transparent;
    border-left: 1px solid black;
    display: grid;
    place-content: center;
    cursor: pointer;
}

</style>
