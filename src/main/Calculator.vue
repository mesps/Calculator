<template>
    <div class="calculator">
        <Display :value="displayValue" @onError="error"/>
        <Button label="AC" firstRow @onClick="clearMemory"/>
        <Button label="%" firstRow @onClick="setOperation"/>
        <Button label="DEL" firstRow @onClick="deleteLast"/>
        <Button label="/" operation @onClick="setOperation"/>
        <Button label="7" @onClick="addDigit"/>
        <Button label="8" @onClick="addDigit"/>
        <Button label="9" @onClick="addDigit"/>
        <Button label="*" operation @onClick="setOperation"/>
        <Button label="4" @onClick="addDigit"/>
        <Button label="5" @onClick="addDigit"/>
        <Button label="6" @onClick="addDigit"/>
        <Button label="-" operation @onClick="setOperation"/>
        <Button label="3" @onClick="addDigit"/>
        <Button label="2" @onClick="addDigit"/>
        <Button label="1" @onClick="addDigit"/>
        <Button label="+" operation @onClick="setOperation"/>
        <Button label="0" double @onClick="addDigit"/>
        <Button label="." @onClick="addDigit"/>
        <Button label="=" operation @onClick="setOperation"/>
    </div>
</template>

<script>
    import Display from "../components/display"
    import Button from "../components/button"

    export default {
        data: function (){
            return {
                displayValue: "0",
                clearDisplay: false,
                operation: null,
                isAnswer: false,
                current: 0
            }
        },
        components: {Button, Display},
        methods: {
            clearMemory() {
                Object.assign(this.$data, this.$options.data())
            },
            setOperation(operation) {

                if(operation === '=') {
                    try {
                        let displayValue = parseFloat(eval(`${this.displayValue}`))
                        displayValue = (displayValue < 1 && displayValue > 0)?
                                        displayValue.toFixed(5) : displayValue

                        this.displayValue = displayValue
                        this.isAnswer = true
                    }
                    catch(e) {
                        this.$emit('onError')
                    }
                }
                else {
                    if(this.displayValue.length === 10) return
                    this.isAnswer = false
                    this.displayValue = this.displayValue + operation
                }
            },
            addDigit(n) {
                if(this.displayValue.length === 10) return
                else if(n === '.' && this.displayValue.includes(".")) return
                else if(this.isAnswer) {
                    this.isAnswer = false
                    this.displayValue = n
                    return
                }

                const clearDisplay = this.displayValue === '0'
                    || this.clearDisplay
                
                const currentValue = clearDisplay ? "" : this.displayValue
                const displayValue = currentValue + n
                
                this.displayValue = displayValue
                this.clearDisplay = false
            },
            deleteLast() {
                let displayValue = this.displayValue.toString()
                
                displayValue = displayValue.substring(0, displayValue.length - 1)
                this.displayValue = displayValue
            },
            error() {
                this.displayValue='ERR. Try Again.'
            }
        }
    }
</script>

<style>
    .calculator {
        height: 320px;
        width: 235px;
        border-radius: 5px;
        overflow: hidden;
        display: grid;
        grid-template-columns: repeat(4, 25%);
        grid-template-rows: 1fr 48px 48px 48px 48px 48px;
    }
</style>