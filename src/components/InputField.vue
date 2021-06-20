<template>
    <div class="input"
    :class="{danger : !isInputValid}" >
    <p class="error-report" v-if="!isInputValid">Please enter a valid Input</p>
        <input type="text" 
        class="input__input-field"
        :placeholder="placeholder" 
        v-model.lazy="inputValue"
        @keyup.enter="inputValueHandler"
        >
        <button class="input__submit" @click="inputValueHandler">
            <svg xmlns="http://www.w3.org/2000/svg" width="11" height="14">
                <path fill="none" stroke="#FFF" stroke-width="3" d="M2 1l6 6-6 6"/>
            </svg>
        </button>
    </div>
</template>

<script>
import { inject, ref, watch } from '@vue/runtime-core'


export default {
    props: {
        placeholder: {
            type: String,
            required: true,
            default: 'Search for any IP address or domain'
        }
    },
    setup() {
        const inputValue = ref('')
        const isInputValid = ref(true)
        const updateCurIP = inject('updateCurIP')

        const inputValueHandler = () => {
            if(/\b(25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])\.(25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])\.(25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])\.(25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])\b/g.test(inputValue.value.trim())){
                isInputValid.value = true
                updateCurIP(inputValue.value)   
                inputValue.value = ''
            } else {
                isInputValid.value = false
                setTimeout(() => {
                    isInputValid.value = true
                }, 2000);
            }
            
        }
        watch(inputValue, () => {
            
        })
        return {
            inputValue,
            isInputValid,
            inputValueHandler
        }
    }
}
</script>

<style lang="scss">
.input {
    height: 3.19rem;
    display: flex;
    width: 88%;
    max-width: 34.69rem;
    margin: 0 auto;
    border-radius: .875rem;
    overflow: hidden;
    position: relative;
    > input {
        transition: padding 200ms ease-out
    }
    &.danger .input__input-field {
        padding-top: .75rem;
    }
    p.error-report {
        position: absolute;

        font-size: .75rem;
        color: hsl(0, 97%, 61%);
        transform: translate(13%, 5%)
    }
    &.danger {
        border: 3px solid hsl(0, 97%, 61%);
    }
}
.input__input-field {
    width: 100%;
    box-sizing: inherit;
    font-family: inherit;
    font-size: inherit;
    padding-left: 1.25rem;
    border: none;
    outline: none;

    &::placeholder {
        color: var(--darkGray);
        font-weight: 400;
    }
    &:active {
        border: none;
        outline: none;
    }
}
.input__submit {
    
    width: 4.25rem;
    background-color: black;
    border: none;
    outline: none;
    transition: background-color 150ms ease;

    &:active {
        border: none;
        outline: none;
        background-color: var(--veryDarkGray)
    }

    &:hover {

        @media (min-width: 800px) {
            background-color: var(--veryDarkGray);
            cursor: pointer;
        }
    }
}
.leaflet-marker-icon {
    cursor: grab !important;
    &:active {
        cursor: grabbing !important;
    }
}
</style>