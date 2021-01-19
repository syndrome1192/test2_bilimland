<template>
  <div>
    <div class="insert">
      <div class="insert__body">
        <form @submit.prevent="takeData">
          <div class="insert__input">
            <label class="insert__label">Название задачи:</label>
            <input type="text" v-model="taskName" class="insert__input" name="task_name" placeholder="Название задачи" />
          </div>
          <div class="insert__input">
            <label class="insert__label">Описание задачи:</label>
            <textarea class="insert__textarea" v-model="taskDescription">Описание задачи</textarea>
          </div>
          <div class="insert__input">
            <button type="submit" class="insert__button">Создать</button>
          </div>
        </form>
      </div>
    </div>
    <div class="filters">
      <form>
        <div class="filters__block">
          <input type="text" v-model="filteredSearch" class="filters__input"  placeholder="Поиск..." />
          <button type="button" @click="sorting" class="filters__button">Фильтровать по алфавиту</button>
        </div>
      </form>
    </div>
    <div class="loader" v-if="loading">
      <div class="loader__body">LOADING...</div>      
    </div>
    <div v-else class="task">
      <div class="task__block">
        <div class="task__header">
          <div class="task__item">Выберите</div>
          <div class="task__item">#</div>
          <div class="task__item">Название задачи</div>
          <div class="task__item">Описание задачи</div>
        </div>        
        <div class="task__content" v-for="task in search" :key="task.id">
          <div class="task__item"><input type="checkbox" :value="task.id" v-model="id" /></div>
          <div class="task__item">{{task.id}}</div>
          <div class="task__item">{{task.taskName}}</div>
          <div class="task__item">{{task.taskDescription}}</div>
        </div>        
        <div class="task__wrap">
          <button type="button" @click="delItem" class="task__button">Удалить</button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'Home',
  data: () => ({
    taskName: '',
    taskDescription: '',
    data: [],
    id: [],
    counter: 0,
    loading: false,
    filteredSearch: '',
    click: 0
  }),  
  computed:{
    search(){
      let filters = this.data.filter( f => {
          return this.filteredSearch == '' || f.taskName.toLowerCase().indexOf(this.filteredSearch.toLowerCase()) !== -1 || f.taskDescription.toLowerCase().indexOf(this.filteredSearch.toLowerCase()) !== -1
        })
      return filters
    }    
  },
  methods: {
    async takeData(){
      this.loading = true
      await new Promise((resolve, reject) => {
        setTimeout(() => {
          this.counter += 1
          this.data.push({id: this.counter, taskName: this.taskName, taskDescription: this.taskDescription})
          resolve(this.data)
        }, 3000)       
      })
      this.taskName = ''
      this.taskDescription = ''
      this.loading = false
    },
    delItem(){
      for(let key in this.id){
       this.data = this.data.filter(uid => uid.id !== this.id[key])       
      }
           
    },
    sorting(){
      let sorts;
      if(this.click % 2 === 0){
        sorts = this.data.sort((a, b) => {
          if (a.taskName < b.taskName)
            return -1
          if (a.taskName > b.taskName)
            return 1
          return 0
        })
      } else {
         sorts = this.data.sort((a, b) => {
          if (a.taskName > b.taskName)
            return -1
          if (a.taskName < b.taskName)
            return 1
          return 0
        })
      }
      

      this.click += 1

      return sorts
    }    
  }
}
</script>
<style lang="scss">
  hr {
    height: 1px;
    background: rgb(205, 205, 206);
    border: none;
  }
  .loader{
    padding-top: 30px;
    &__body{
      text-align: center;      
    }
  }
  .insert{
    &__body{
      max-width: 635px;
      margin: 0px auto;
      background: #fff;
      border: 1px solid rgb(206, 205, 205);
    }

    &__input{
      margin-top: 10px;
    }

    &__input{
      width: 100%;
      padding: 12px 20px;
      margin: 8px 0;
      box-sizing: border-box;
    }

    &__textarea{
      width: 100%;
      height: 150px;
      padding: 12px 20px;
      box-sizing: border-box;
      border: 2px solid #ccc;
      border-radius: 4px;
      background-color: #f8f8f8;
      resize: none;
    }

    &__button{
      background-color: #4CAF50;
      border: none;
      color: white;
      padding: 16px 32px;
      text-decoration: none;
      
      cursor: pointer;
    }

    &__label{
      font-size: 18px;
    }
  }

  .task{
    margin-top: 30px;
    padding: 0;

    &__block{
      max-width: 1200px;
      margin: 0px auto;
      background: #fff;      
      border: 1px solid rgb(206, 205, 205);

    }

    &__header{
      display: flex; 
      justify-content: space-between;     
      padding: 10px;
      color: #fff;
      background: rgb(6, 243, 38);
      align-items: center;
      
      
    }    

    &__content{
      display: flex;
      justify-content: space-between;
      padding: 10px;
      align-items: center;
    }

    &__content:nth-child(2n){
      background: rgba(230, 230, 230, 0.445);
    }

    &__item {
      font-size: 16px; 
      width: 65%; 
      text-align: center;    
    }

    &__wrap{
      margin: 20px;
    }

    &__button{
      background-color: #d62d27;
      border: none;
      color: white;
      padding: 10px 22px;
      text-decoration: none;
      
    }
  }

  .filters {
    &__block{
      display: flex;
      max-width: 800px;
      margin: 0px auto;
      margin-top: 20px;
      background: #fff;
      padding: 10px;
      border: 1px solid rgb(206, 205, 205);
    }

    &__input{
      width: 80%;
      padding: 12px 20px;
      margin: 8px 0;
      box-sizing: border-box;
    }

    &__button{
      background-color: #4CAF50;
      border: none;
      color: white;
      padding: 16px 20px;
      text-decoration: none;
      margin-left: 10px;
      cursor: pointer;
    }
  }
</style>


