<template>
    <!-- Модальное окно, отображается при modalVisible === true -->
    <div v-if="modalVisible" class="modal" @click="closeModal">
        <div class="modal__content" @click.stop>
            <!-- Кнопка закрытия модального окна -->
            <button class="modal__close" @click="closeModal">
                <svg width="43" height="43" viewBox="0 0 43 43" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M29.4573 16.4852C30.2244 15.7703 30.2667 14.5688 29.5517 13.8017C28.8367 13.0346 27.6353 12.9924 26.8682 13.7073L21.3123 18.8855L16.1341 13.3297C15.4192 12.5626 14.2177 12.5203 13.4506 13.2352C12.6835 13.9502 12.6413 15.1517 13.3562 15.9188L18.5344 21.4746L12.9786 26.6528C12.2115 27.3678 12.1692 28.5692 12.8841 29.3363C13.5991 30.1034 14.8006 30.1457 15.5677 29.4307L21.1235 24.2525L26.3017 29.8084C27.0167 30.5755 28.2181 30.6178 28.9852 29.9028C29.7523 29.1878 29.7946 27.9864 29.0796 27.2193L23.9014 21.6634L29.4573 16.4852Z" fill="#58595B"/>
                    <path fill-rule="evenodd" clip-rule="evenodd" d="M0.332275 21.5691C0.332275 10.0342 9.68314 0.683372 21.218 0.683372C32.7529 0.683372 42.1038 10.0342 42.1038 21.5691C42.1038 33.104 32.7529 42.4549 21.218 42.4549C9.68314 42.4549 0.332275 33.104 0.332275 21.5691ZM21.218 38.6575C11.7804 38.6575 4.12968 31.0068 4.12968 21.5691C4.12968 12.1315 11.7804 4.48078 21.218 4.48078C30.6557 4.48078 38.3064 12.1315 38.3064 21.5691C38.3064 31.0068 30.6557 38.6575 21.218 38.6575Z" fill="#58595B"/>
                </svg>
            </button>

            <div class="modal__descr">
                Отказаться от выполнения заказа
                <span>№ У00017711</span>
            </div>
            <!-- Заголовок модального окна -->
            <h2 class="modal__title">{{ title }}</h2>

            <!-- Контент с радиокнопками -->
            <div class="modal__body">
                <div class="modal__radio-button" v-for="item in content">
                    <input type="radio" name="radio" :id="'id-' + item.id">
                    <label :for="'id-' + item.id" class="modal__radio-button__radio-square"></label>
                    <label class="modal__radio-button__radio-label" :for="'id-' + item.id">{{ item.text }}</label>
                </div>
            </div>

            <!-- Кнопка отправки -->
            <div class="modal__line-button">
                <button class="modal__button">ОТПРАВИТЬ</button>
            </div>
        </div>
    </div>
</template>

<script>
import { defineComponent, computed, onMounted, onUnmounted } from 'vue';

export default defineComponent({
    name: 'Modal',
    props: {
        isOpen: {
            type: Boolean,
            required: true,
        },
        onClose: {
            type: Function,
            required: true,
        },
        title: {
            type: String,
            default: '',
        },
        content: {
            type: [String, Object],
            default: '',
        },
    },
    setup(props) {
        // Вычисляемое свойство для контроля видимости модального окна
        const modalVisible = computed(() => props.isOpen);

        // Функция закрытия модального окна
        const closeModal = () => {
            props.onClose();
        };

        // Закрытие по нажатию клавиши Escape
        const handleEscape = (event) => {
            if (event.key === 'Escape') {
                closeModal();
            }
        };

        // Добавляем обработчик события при монтировании компонента
        onMounted(() => {
            document.addEventListener('keydown', handleEscape);
        });

        // Удаляем обработчик события при размонтировании компонента
        onUnmounted(() => {
            document.removeEventListener('keydown', handleEscape);
        });

        return { modalVisible, closeModal };
    },
});
</script>

<style lang="scss" scoped>
.modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;

    &__content {
        position: relative;
        width: 90%;
        max-width: 615px;
        padding: 60px;
        color: #151A22;
        background: #fff;
        border-radius: 45px 45px 0 0;
        box-shadow: 1.42px 1.42px 11.39px 2.85px #B0B0B040;
        animation: fadeIn 0.3s ease-in-out;
    }

    &__close {
        position: absolute;
        top: 25px;
        right: 30px;
        display: flex;
        align-items: center;
        justify-content: center;
        background: none;
        border: none;
        cursor: pointer;
        transition: 0.3s;
        &:hover {
            opacity: 0.7;
        }
        svg {
            width: 42px;
            height: 42px;
        }
    }
    &__descr {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        margin-bottom: 40px;
        font-weight: 400;
        font-size: 25px;
        text-transform: uppercase;
        color: #151A22;
        span {
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 700;
        }
    }
    &__title {
        margin-bottom: 55px;
        font-size: 25px;
        line-height: 28px;
        font-weight: 900;
        text-transform: uppercase;
        color: #343330;
    }

    &__radio-button {
        display: flex;
        align-items: center;
        margin-bottom: 45px;
        font-size: 25px;
        line-height: 28px;
        font-weight: 400;
        text-transform: uppercase;
        input {
            display: none;
            &:checked + .modal__radio-button__radio-square {
                &::after {
                    opacity: 1;
                }
            }
        }
        &__radio-square {
            display: flex;
            align-items: center;
            justify-content: center;
            width: 41px;
            height: 41px;
            max-width: 41px;
            max-height: 41px;
            min-width: 41px;
            min-height: 41px;
            border-radius: 7px;
            border: 3px solid #58595B;
            background: #fff;
            transition: 0.3s;
            cursor: pointer;
            &:hover {
                &::after {
                    opacity: 0.3;
                }
            }
            &::after {
                content: '';
                width: 27px;
                height: 27px;
                border-radius: 3px;
                background: #CD10FF;
                transition: 0.3s;
                opacity: 0;
            }
        }
        &__radio-label {
            padding-left: 32px;
            cursor: pointer;
        }
    }
    &__line-button {
        position: absolute;
        z-index: 1;
        bottom: 0;
        left: 0;
        display: flex;
        align-items: center;
        justify-content: center;
        width: 100%;
        height: 145px;
        background: #fff;
    }
    &__button {
        width: 460px;
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 0 20px;
        font-size: 20px;
        line-height: 60px;
        text-transform: uppercase;
        border: 1.5px solid #CD10FF;
        border-radius: 11px;
        background: transparent;
        color: #000;
        cursor: pointer;
        transition: 0.3s;
        &:hover {
            opacity: 0.7;
        }
    }
    &__body {
        max-height: 400px;
        overflow: auto;
        margin-bottom: 145px;
    }
}

@keyframes fadeIn {
    from {
        opacity: 0;
        transform: scale(0.9);
    }
    to {
        opacity: 1;
        transform: scale(1);
    }
}
</style>
