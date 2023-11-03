 <script setup>

import { ref } from 'vue';


const newTask = ref('');
const newContent = ref('');
const tasks = ref([]);
const editedTask = ref({ title: '', content: '', index: -1 });
const showModal = ref(false);
 const showEditSection=ref(false);
 const isSaving = ref(false);

const addTask = () => {
  if (newTask.value.trim() === '') {
    return;
  }
  tasks.value.push({ title: newTask.value, content: newContent.value, completed: false });
  newTask.value = '';
  newContent.value='';
};

const removeTask = (index) => {
  tasks.value.splice(index, 1);
};

const showEditModal = (index) => {
  editedTask.value = { title: tasks.value[index].title, content: tasks.value[index].content, index };
  showModal.value = true;
  showEditSection.value=true;
  isSaving.value = false;
};

const saveEditedTask = () => {
  if (editedTask.value.index !== -1) {
    tasks.value[editedTask.value.index].title = editedTask.value.title;
    tasks.value[editedTask.value.index].content = editedTask.value.content;
  }
  editedTask.value = { title: '', content: '', index: -1 };
  showModal.value = false;
  showEditSection.value=false;
  isSaving.value = true;
};

const toggleTask = (index) => {
  tasks.value[index].completed !=tasks.value[index].completed;
};

const getIncompleteTasksCount = () => {
  return tasks.value.filter((task) => !task.completed).length;
};

</script>

<template>
  <div style="text-align: center;width: 90vw;"> 
    <h1 class="text-center">Ma ToDo List</h1>
  </div>
  
  <br>
    <div style="text-align: center;  width: 40vw; margin-left: 30%;">
    
      <div style="font-size: large;">
        <label style="display: inline;">Tâches non complétées :</label>
        <h3 style="display: inline;">{{ getIncompleteTasksCount() }}</h3>
      </div>

        <div style="width: 40vw; ">
          <input v-model="newTask"  placeholder="Nouvelle tâche" class="form-control" >
          <input v-model="newContent"  placeholder="Contenu de la tâche" class="form-control" >
          <button @click="addTask" class="btn btn-primary">Ajouter</button>
        </div>
    </div>
    <br>
    <br>
    <div style="margin-top: 40px;margin-left: 30%;" >
      <div  >
        <div style="width: 40vw; ">
          <h3>Tâches</h3>
          <ul class="list-group">
            <li
              class="list-group-item d-flex justify-content-between align-items-center"
              v-for="(task, index) in tasks"
              :key="index"
            >
            <span style="text-align: center;">
                Titre: {{ task.title }} <br> Taches: {{ task.content }} <br>
                <button @click="showEditModal(index),showEditModal.value(true)" class="btn btn-primary btn-sm">Modifier</button>
                <button @click="removeTask(index)" class="btn btn-danger btn-sm" >Supprimer</button>
            </span>
            <br>
              <div>
                <div class="form-check form-switch">
                  <input
                    type="checkbox"
                    class="form-check-input"
                    v-model="task.completed"
                    @change="toggleTask(index)"
                  />
                  <label class="form-check-label">{{ task.completed ? 'Terminée' : 'Non terminée' }}</label>
                </div>
                
              </div>
            </li>
          </ul>
        </div>
      </div>
      
    </div>
    <div class="modal-background" :class="{ 'scale-up': showEditSection }" v-if="showEditSection">
  <div class="modal-content">
    <div class="form-group">
      <label for="editedTaskTitle">Titre</label>
      <input v-model="editedTask.title" id="editedTaskTitle" class="form-control">
    </div>
    <div class="form-group">
      <label for="editedTaskContent">Contenu</label>
      <textarea v-model="editedTask.content" id="editedTaskContent" class="form-control" rows="5"></textarea>
    </div>
    <button @click="saveEditedTask" :class="{ 'scale-down': isSaving }" class="btn btn-primary">Sauvegarder</button>
  </div>
</div>

    

   
</template>
<style>

.scale-up {
  animation: scaleUp 0.5s cubic-bezier(0.165, 0.840, 0.440, 1.000) forwards;
}
.scale-down {
  animation: scaleDown 0.8s cubic-bezier(0.165, 0.840, 0.440, 1.000) forwards;
}
.modal-background {
  color: #fff;
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(0, 0, 0, 0.7); /* Fond noir semi-transparent */
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 999; /* N'oubliez pas d'ajuster la valeur z-index si nécessaire */
}

.modal-content {
  background: #fff; /* Couleur de fond de la section d'édition */
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.5);
}

@keyframes scaleUp {
  from {
    transform: scale(0);
  }
  to {
    transform: scale(1);
  }
}

@keyframes scaleDown {
  from {
    transform: scale(1);
  }
  to {
    transform: scale(0);
  }
}
</style>



