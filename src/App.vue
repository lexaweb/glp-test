<template>
    <!-- Кнопка, при клике на которую откроется модальное окно -->
    <button class="button-test" @click="openModal">Открыть модальное окно</button>

    <!-- Модальное окно -->
    <modal
        :isOpen="isModalOpen"
        :onClose="closeModal"
        title="Расскажите, почему отказались от выполнения заказа?"
        :content="content"
    ></modal>
    <!-- :isOpen="isModalOpen" Передаем состояние, открыто ли модальное окно -->
    <!-- :onClose="closeModal" Передаем функцию для закрытия модального окна -->
    <!-- title="Расскажите, почему отказались от выполнения заказа?" Заголовок модального окна -->
    <!-- :content="content" Передаем данные для контента в модальном окне -->
</template>

<script>
import { ref } from 'vue';  // Импортируем функцию ref для создания реактивных переменных
import Modal from "./components/Modal.vue";  // Импортируем компонент модального окна

export default {
    components: { Modal },  // Регистрируем компонент Modal

    setup() {
        // Данные для контента модального окна
        const content = ref([
            { id: 1, text: 'Не подходит дата или время' },
            { id: 2, text: 'Не подходит тоннаж груза' },
        ]);

        // Переменная для отслеживания состояния модального окна (открыто/закрыто)
        const isModalOpen = ref(false);

        // Функция для открытия модального окна
        const openModal = () => {
            isModalOpen.value = true;  // Устанавливаем состояние модального окна как открытое
        };

        // Функция для закрытия модального окна
        const closeModal = () => {
            isModalOpen.value = false;  // Устанавливаем состояние модального окна как закрытое
        };

        // Возвращаем все переменные и функции, чтобы они были доступны в шаблоне
        return {
            content,
            isModalOpen,
            openModal,
            closeModal
        };
    }
}
</script>

<style lang="scss">
:root {
    color-scheme: none;
}
* {
    box-sizing: border-box;
}
button {
    padding: 0;
    &:focus,
    &:focus {
        outline: none;
    }
}
.button-test {
    padding: 20px;
    background: #fff;
}

</style>
