

<template>
  <div class="py-10 max-w-[1200px] mx-auto">
    <div>
      <h1 class="text-3xl font-semibold text-center text-white font-body">TODO ENERGEEK</h1>
      <section class="py-20 ">
        <div class="card bg-slate-100 shadow-[5px_5px_0px_0px] shadow-teal-500 p-5 rounded-md border border-teal-600">
          <div class="flex justify-between mb-5">
            <div>
              <h1 class="text-lg font-semibold text-dark">{{todo == 1 ? 'Todo' : 'Update Progress'}}</h1>
            </div>
            <div>
              <button class="px-8 py-2 text-white bg-indigo-600 rounded-md " type="button" @click="switchBtn">Update Progress</button>
            </div>
          </div>
          <!-- <div class="grid grid-flow-col gap-5"> -->
            <div class="w-full overflow-x-auto">
              <table class="min-w-full border-collapse table-auto">
                <thead>
                  <tr class="text-sm leading-normal text-gray-700 uppercase">
                    <th class="px-6 py-3 text-left">No</th>
                    <th class="px-6 py-3 text-left">Project Name</th>
                    <th class="px-6 py-3 text-left">Task</th>
                    <th class="px-6 py-3 text-left">Action</th>
                  </tr>
                </thead>
                <tbody class="text-sm font-light text-gray-600">
                  <template v-for="(task, index) in tasks" :key="index">
                    <tr class="text-center border-b border-gray-200 hover:bg-gray-100">
                      <td class="px-6 py-3 text-left">{{ index + 1 }}</td>
                      <td class="px-6 py-3 text-left">
                        <input type="text" class=" border border-gray-300 text-slate-700 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 " v-model="task.projectName" />
                      </td>
                      <td class="px-6 py-3 text-left">
                        <input type="text" class=" border border-gray-300 text-slate-700 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 " v-model="task.taskName" />
                      </td>
                      <td class="px-6 py-3 text-left">
                        <!-- Add your action buttons here -->
                        <button class="px-4 py-2 font-bold text-white bg-red-500 rounded hover:bg-red-700" @click="deleteTask(task.id)">
                          Delete
                        </button>
                      </td>
                    </tr>
                  </template>
                </tbody>
              </table>
              <div class="flex justify-between mt-5">
                <button class="px-4 py-2 font-bold text-white rounded bg-emerald-500 hover:bg-emerald-700" @click="addTask">
                  Tambah
                </button>
                <button class="px-4 py-2 font-bold text-white rounded bg-emerald-500 hover:bg-emerald-700" @click="generate">
                  Generate
                </button>
              </div>
            </div>

            <div class="w-full py-5 overflow-auto" v-if="isGenerate">
              <div class="bg-slate-100 shadow-[5px_5px_0px_0px] shadow-teal-500 p-5 rounded-md border border-teal-600">
                <div class="flex justify-between mb-5">
                  <h1 class="text-2xl font-semibold text-dark">Generate {{todo == 1 ? 'Todo' : "Update Progress"}}</h1>
                  <h1 class="px-4 py-2 font-medium text-white bg-teal-500 rounded cursor-pointer font-body text-md" @click="copyToClipboard">Copy Text</h1>
                </div>
                <div class="p-5 border rounded bg-slate-200 border-slate-300" id="mytodo">
                  <h1 class="text-lg font-semibold text-dark font-body">{{todo == 1 ? 'mytodolist:' : 'myprogresslist:'}}</h1>
                  <template v-for="task in tasks" :key="task.id">
                    <h1 class="text-lg font-semibold text-dark font-body">{{task.id}}. {{task.projectName}} - {{task.taskName}}</h1>
                  </template>
                </div>
              </div>
            </div>
            
          
       </div>
      </section>
    </div>
  </div>

</template>
<script setup>
import { ref, computed, onMounted } from 'vue'
import {useToast} from 'vue-toastification'
 onMounted(()=>{

 })

  const toast = useToast()
 const projectName = ref('')
 const taskName = ref('')
 const btn = ref (true);
 const todo = ref(true);
 const tasks = ref([]);
 const isGenerate = ref(false);
 
 const switchBtn = () => {
   todo.value = !todo.value;
 }
 const addTask = () => {
   tasks.value.push({
     id : tasks.value.length + 1,
     projectName : projectName.value,
     taskName : taskName.value
   })
 }
 const deleteTask = (id) => {
   tasks.value = tasks.value.filter((task) => task.id !== id)
 }

 const generate = () => {
   if(tasks.value.length == 0){
     toast.error('Data Tidak Boleh Kosong')
     return;
   }
   isGenerate.value = !isGenerate.value
   console.log(tasks.value)
 }

 const copyToClipboard = () => {
  const mytodoElement = document.getElementById('mytodo');
      const range = document.createRange();
      range.selectNodeContents(mytodoElement);
      const selection = window.getSelection();
      selection.removeAllRanges();
      selection.addRange(range);

      try {
        document.execCommand('copy');
        toast.success('Text copied to clipboard');
      } catch (err) {
       toast.error('Failed to copy text');
      }

      selection.removeAllRanges();
 }


</script>
