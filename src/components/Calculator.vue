<script>
export default {
    name: 'Calculator',
    data() {
        return {
            total: '0',
            buttonsData: [
                {title: '7', action: '7'},
                {title: '8', action: '8'},
                {title: '9', action: '9'},
                {title: 'Del', action: 'clearLast', class: 'secondary --short'},
                {title: '4', action: '4'},
                {title: '5', action: '5'},
                {title: '6', action: '6'},
                {title: '+', action: '+'},
                {title: '1', action: '1'},
                {title: '2', action: '2'},
                {title: '3', action: '3'},
                {title: '-', action: '-'},
                {title: '.', action: '.'},
                {title: '0', action: '0', class: 'col-span-2'},
                {title: '/', action: '/'},
                {title: '×', action: '*'},
                {title: 'Reset', action: 'clear', class: 'secondary'},
                {title: '=', action: 'result', class: 'primary'},
            ],
        }
    },
    methods: {
        handleClick(value) {
            switch (value) {
                case 'clearLast':
                    if (this.total.length <= 1) {
                        this.total = '0';
                    } else {
                        this.total = this.total.slice(0, -1)
                    }

                    break;

                case 'clear':
                    this.total = '0';
                    break;

                case 'result':
                    if (this.total.slice(-1).match(/[-+*/]/)) {
                        this.total = this.total.slice(0, -1);
                    }

                    this.total = eval(this.total);

                    break;

                default:
                    if (this.total === '0' && !value.match(/[-+*/]/)) {
                        // Prevents adding operators on the beginning
                        this.total = value;

                        break;
                    } else if (
                        this.total &&
                        value.match(/[-+*/]/) &&
                        this.total.slice(-1).match(/[-+*/]/)
                    ) {
                        // Prevents adding multiple operators
                        if (value !== this.total.slice(-1)) {
                            // If the last character is an operator, replace the previous one
                            this.total = this.total.slice(0, -1) + value;
                        }

                        break;
                    }

                    this.total += value;
                    break;
            }
        }
    },
}
</script>

<template>
    <div class="calculator">
        <div class="header">
            <div class="logo">calc</div>
        </div>
        <div class="output">
            {{ total }}
        </div>

        <div class="buttons">
            <div v-for="button in buttonsData"
                 @click="handleClick(button.action)"
                 :class="button.class"
                 class="button">
                {{ button.title }}
            </div>
        </div>
    </div>
</template>

<style lang='scss'>

.calculator {
    display: flex;
    flex-direction: column;
    gap: 24px;
    margin: 32px;
    padding: 32px;
    width: 100%;
    min-width: 327px;
    max-width: 540px;
    border-radius: $border-radius;
    background-color: $bg-calculator;

    .logo {
        color: #FFF;
        font-size: 2rem;
        font-weight: bold;
    }

    .output {
        display: flex;
        justify-content: end;
        align-items: center;
        padding: 0 32px;
        height: 128px;
        font-size: 3.5rem;
        font-weight: bold;
        overflow: hidden;
        background-color: $bg-output;
        color: #FFF;
        border-radius: $border-radius;
    }

    .buttons {
        display: flex;
        flex-wrap: wrap;
        gap: 24px;
    }

    .button {
        display: flex;
        flex: 0 1 25%;
        align-items: center;
        justify-content: center;
        height: 64px;
        width: 100%;
        max-width: 101px;
        color: $text-btn;
        font-size: 2.5rem;
        font-weight: bold;
        text-transform: uppercase;
        user-select: none;
        -webkit-user-select: none;
        background-color: $bg-btn;
        box-shadow: $bg-btn-shadow;
        border-radius: $border-radius;
        cursor: pointer;

        &.primary,
        &.secondary {
            font-size: 1.75rem;
            color: #FFF;
        }

        &.primary {
            background-color: $bg-btn-primary;
            box-shadow: $bg-btn-primary-shadow;
            flex-basis: 50%;
            max-width: 227px;
        }

        &.secondary {
            background-color: $bg-btn-secondary;
            box-shadow: $bg-btn-secondary-shadow;

            &:not(.--short) {
                flex-basis: 50%;
                max-width: 225px;
            }
        }

        &:hover {
            opacity: .8;
        }
    }
}

</style>
