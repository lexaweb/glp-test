Как использовать компонент Modal:

1. Импортируйте компонент в файл, где вы хотите его использовать:

   import Modal from "./components/Modal.vue";

2. Определите состояние модального окна и контент, который будет передаваться в него:

   import { ref } from 'vue';

   const isModalOpen = ref(false);
   const content = ref([
       { id: 1, text: 'Не подходит дата или время' },
       { id: 2, text: 'Не подходит тоннаж груза' }
   ]);

   const openModal = () => {
       isModalOpen.value = true;
   };

   const closeModal = () => {
       isModalOpen.value = false;
   };
   

3. Добавьте компонент Modal в шаблон и передайте необходимые пропсы:

   <template>
       <button @click="openModal">Открыть модальное окно</button>

       <Modal
           :isOpen="isModalOpen"
           :onClose="closeModal"
           title="Расскажите, почему отказались от выполнения заказа?"
           :content="content"
       />
   </template>
