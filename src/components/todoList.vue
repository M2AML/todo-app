<script setup>
import { computed, ref } from "vue";
import { v4 as uuidv4 } from "uuid";
import crossIcon from "../assets/images/icon-cross.svg";
import { VueDraggable } from "vue-draggable-plus";

const tasksList = ref([
  {
    text: "Complete online Javascript course",
    completed: true,
    id: uuidv4(),
  },
  {
    text: "Jog around a park 3x",
    completed: false,
    id: uuidv4(),
  },
  {
    text: "10 minutes meditation",
    completed: false,
    id: uuidv4(),
  },
  {
    text: "Read for 1 hour",
    completed: false,
    id: uuidv4(),
  },
  {
    text: "Pick up groceries",
    completed: false,
    id: uuidv4(),
  },
  {
    text: "Completed Todo App on Frontend Mentor",
    completed: true,
    id: uuidv4(),
  },
]);
const tasksText = ref("");
const filter = ref("all");
const newTask = () => {
  tasksList.value.push({
    text: tasksText.value,
    completed: false,
    id: uuidv4(),
  });
  tasksText.value = "";
};

const sortedTasks = computed(() => {
  const taskFiltered = tasksList.value;
  switch (filter.value) {
    case "active":
      return taskFiltered.filter((t) => t.completed === false);
    case "completed":
      return taskFiltered.filter((t) => t.completed === true);
    default:
      return taskFiltered;
  }
});

const deleteTask = (taskID) => {
  const index = tasksList.value.findIndex((i) => i.id === taskID);
  if (index !== -1) {
    tasksList.value.splice(index, 1);
  }
};

const clearCompleted = () => {
  const clearTasksDone = tasksList.value.filter(
    (task) => task.completed !== true
  );
  tasksList.value = clearTasksDone;
};

const tasksLeft = computed(() => {
  return tasksList.value.filter((task) => task.completed !== true).length;
});
</script>

<template>
  <section class="form_container">
    <form @submit.prevent="newTask">
      <button class="btn_submit"></button>
      <input
        required
        type="text"
        v-model="tasksText"
        placeholder="Create a new todo..."
      />
    </form>
    <ul>
      <VueDraggable ref="el" v-model="tasksList">
        <li
          v-for="task in sortedTasks"
          :key="task.id"
          :class="{ done: task.completed }"
        >
          <label>
            <input type="checkbox" v-model="task.completed" />
            {{ task.text }}
          </label>
          <button class="delete_todo" @click="deleteTask(task.id)">
            <img :src="crossIcon" alt="cross icon" />
          </button>
        </li>
      </VueDraggable>
    </ul>
    <div class="action_container">
      <p class="item_left">
        {{ tasksLeft }}
        item{{ tasksLeft > 1 ? "s" : "" }} left{{ tasksLeft > 1 ? "s" : "" }}
      </p>
      <div class="filter_btn_container">
        <input
          type="radio"
          name="filter"
          id="all"
          value="all"
          v-model="filter"
        />
        <label for="all">All</label>
        <input
          type="radio"
          name="filter"
          id="active"
          value="active"
          v-model="filter"
        />
        <label for="active">Active</label>
        <input
          type="radio"
          name="filter"
          id="completed"
          value="completed"
          v-model="filter"
        />
        <label for="completed">Completed</label>
      </div>
      <button class="btn_clear" @click="clearCompleted">Clear Completed</button>
    </div>
  </section>
</template>

<style scoped>
.form_container {
  position: relative;
  width: 100%;
  font-weight: 400;
  box-shadow: rgba(38, 38, 43, 0.2) 0px 7px 29px 0px;
}

form {
  display: flex;
  align-items: center;
  margin-bottom: 25px;
  transition: all 0.5s ease;
}

form input {
  all: unset;
  padding: 5px;
  border: none;
  width: 100%;
  height: 25px;
  transition: all 0.5s ease;
}

.btn_submit {
  all: unset;
  width: 25px;
  height: 22px;
  margin: 15px;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.5s ease;
}

ul {
  list-style: none;
  transition: all 0.5s ease;
}
li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  padding: 10px 15px;
  transition: all 0.5s ease;
}
li label {
  cursor: pointer;
  display: flex;
  align-items: center;
  width: 100%;
}
input[type="checkbox"] {
  position: relative;
  appearance: none;
  border-radius: 50%;
  height: 25px;
  width: 25px;
  border: 1px solid var(--border-light);
  margin-right: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
}
input[type="checkbox"]:checked {
  background-image: url("../assets/images/icon-check.svg"),
    linear-gradient(120deg, hsl(192, 100%, 67%), hsl(280, 87%, 65%));
  background-repeat: no-repeat;
  background-position: center;
  border: none;
}

.done {
  text-decoration: line-through;
}

.delete_todo {
  all: unset;
  opacity: 0;
  transition: all 0.5s ease;
}
li:hover .delete_todo {
  opacity: 1;
}
.action_container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 15px;
  background-color: var(--background-dark);
  color: var(--text-dark-2);
  transition: all 0.5s ease;
}
.filter_btn_container {
  transition: all 0.5s ease;
}
.filter_btn_container label {
  margin-right: 10px;
  cursor: pointer;
  transition: all 0.5s ease;
}
.filter_btn_container input {
  display: none;
}

.filter_btn_container input:checked + label {
  color: hsl(220, 98%, 61%);
}

.btn_clear {
  all: unset;
  cursor: pointer;
  transition: all 0.5s ease;
}

/* Dark Theme */
#dark {
  color: var(--text-dark-1);
}
#dark form {
  background-color: var(--background-dark);
}
#dark ul {
  background-color: var(--background-dark);
}
#dark .action_container {
  background-color: var(--background-dark);
}
#dark .action_container .filter_btn_container {
  background-color: var(--background-dark);
}
#dark .btn_submit {
  border: 1px solid hsl(233, 14%, 35%);
}
#dark li {
  border-bottom: 1px solid hsl(233, 14%, 35%);
}
#dark .done {
  color: var(--task-done-dark);
}
#dark .action_container,
.btn_clear {
  color: var(--text-dark-2);
}
#dark .btn_clear:hover {
  color: var(--text-dark-hover);
}
#dark .filter_btn_container label:hover {
  color: var(--text-dark-hover);
}

/* Light Theme */

#light {
  color: var(--text-light-1);
}
#light form {
  background-color: var(--background-light);
}
#light ul {
  background-color: var(--background-light);
}
#light .action_container {
  background-color: var(--background-light);
}
#light .action_container .filter_btn_container {
  background-color: var(--background-light);
}
#light .btn_submit {
  border: 1px solid var(--border-light);
}
#light li {
  border-bottom: 1px solid var(--border-light);
}
#light .done {
  color: var(--text-light-2);
}
#light .action_container,
.btn_clear {
  color: var(--text-light-2);
}
#light .btn_clear:hover,
.filter_btn_container label:hover {
  color: var(--text-light-hover);
}

@media screen and (max-width: 440px) {
  .btn_submit {
    height: 20px;
    margin: 12px;
  }
  ul {
    font-size: 0.9rem;
  }
  li {
    padding: 10px 12px;
  }
  input[type="checkbox"] {
    height: 20px;
    width: 20px;
  }
  .action_container {
    font-size: 0.9rem;
    padding: 10px 12px;
  }
  .filter_btn_container {
    position: absolute;
    bottom: -60px;
    width: 100%;
    margin-left: -12px;
    text-align: center;
    padding: 12px 0;
    border-radius: 4px;
    box-shadow: rgba(38, 38, 43, 0.2) 0px 7px 29px 0px;
  }
}
</style>
