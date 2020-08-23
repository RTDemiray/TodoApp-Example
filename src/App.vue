<template>
  <main id="todoList">

    <h1>
      Yapılacaklar Listesi
      <span>Yapılacaklar listesini işaretleyin.</span>
    </h1>

    <ul v-if="items.length">

    <li v-for="(item, index) in items" :key="index" :class="{'done': item.done}">
        <span class="label">{{item.title}}</span>
        <div class="actions">
            <button class="btn-picto" type="button" @click="changeStatus(index)">
            <i aria-hidden="true" class="material-icons">{{item.done ? 'check_box' : 'check_box_outline_blank'}}</i>
            </button>
            <button @click="removeItem(index)" class="btn-picto" type="button" aria-label="Delete" title="Sil">
            <i aria-hidden="true" class="material-icons">delete</i>
            </button>
        </div>
    </li>

    </ul>

    <p v-else class="emptyList">Yapılacaklar listeniz boş.</p>

    <form @submit.prevent="addNewItem">
      <input type="text" placeholder="Yapılacaklar listesine ekle" name="newItem" id="newItem" v-model="newItemTitle" />
      <button type="submit">Ekle</button>
    </form>

  </main>
</template>

<script>
export default {
    data(){ //kullanacağımız verileri tanımlıyoruz
        return {
            newItemTitle: '',
            items: []
        }
    },
    methods: {
        addNewItem(){
            if(this.newItemTitle === ''){
                return; //işlemi sonlandır
            }

            this.items.push({ //dizi içerisine title ve done elemanlarını ekle
                title: this.newItemTitle,
                done: false
            });

            this.newItemTitle = ''; //veri eklenildiği zaman text alanını boşalt
        },
        removeItem(index){
            this.items.splice(index, 1); //gelen index değerini sil
        },
        changeStatus(index){
            const item = this.items[index];
            item.done = !item.done; //gelen item değerinin tersini al
        }
    },
    mounted(){ //uygulama başladığında çalışır
        const initItems = localStorage.getItem('todo');
        const items = JSON.parse(initItems ? initItems : '[]'); //gelen todo boş ise boş dizi döndür
        this.items = items;
    },
    watch: { //değişiklikleri izler
        items:{
            deep: true,
            handler(){
                const items = this.items;
                localStorage.setItem('todo',JSON.stringify(items)); //herhangi bir değişiklik olduğunda items dizisini todo'ya json formatta çevir ve ata.
            }
        }
    }
}
</script>


<style>
  * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
  }
  html, body {
      background: #f7f1f1;
      font-size: 1.1rem;
      font-family: 'Quicksand', sans-serif;
      height: 100%;
  }
  @keyframes strikeitem {
      to {
          width: calc(100% + 1rem);
      }
  }
  #todoList {
      margin: 4rem auto;
      padding: 2rem 3rem 3rem;
      max-width: 500px;
      background: #FF6666;
      color: #FFF;
      box-shadow: -20px -20px 0px 0px rgba(100, 100, 100, .1);
  }
  #todoList h1 {
      /*text-align:center;*/
      font-weight: normal;
      font-size: 2.1rem;
      letter-spacing: 0.05em;
      border-bottom: 1px solid rgba(255, 255, 255, .3);
  }
  #todoList h1 span {
      display: block;
      font-size: 0.8rem;
      margin-bottom: 0.7rem;
      margin-left: 3px;
      margin-top: 0.2rem;
  }
  #todoList .emptyList {
      margin-top: 2.6rem;
      text-align: center;
      letter-spacing: .05em;
      font-style: italic;
      opacity: 0.8;
  }
  #todoList ul {
      margin-top: 2.6rem;
      list-style: none;
  }
  #todoList .todoList-move {
      transition: transform 1s;
      outline: none;
  }
  #todoList li {
      display: flex;
      margin: 0 -3rem 4px;
      padding: 1.1rem 3rem;
      justify-content: space-between;
      align-items: center;
      background: rgba(255, 255, 255, 0.1);
  }
  #todoList .actions {
      flex-shrink: 0;
      padding-left: 0.7em;
  }
  #todoList .label {
      position: relative;
      transition: opacity .2s linear;
  }
  #todoList .done .label {
      opacity: .6;
  }
  #todoList .done .label:before {
      content: '';
      position: absolute;
      top: 50%;
      left: -.5rem;
      display: block;
      width: 0%;
      height: 1px;
      background: #FFF;
      animation: strikeitem .3s ease-out 0s forwards;
  }
  #todoList .btn-picto {
      border: none;
      background: none;
      -webkit-appearance: none;
      cursor: pointer;
      color: #FFF;
      outline: none;
  }
  /* FORM */
  form {
      margin-top: 3rem;
      display: flex;
      flex-wrap: wrap;
  }
  form label {
      min-width: 100%;
      margin-bottom: .5rem;
      font-size: 1.3rem;
  }
  form input {
      flex-grow: 1;
      border: none;
      background: #f7f1f1;
      padding: 0 1.5em;
      font-size: initial;
  }
  form button {
      padding: 0 1.3rem;
      border: none;
      background: #FF6666;
      color: white;
      text-transform: uppercase;
      font-weight: bold;
      border: 1px solid rgba(255, 255, 255, .3);
      margin-left: 5px;
      cursor: pointer;
      transition: background .2s ease-out;
  }
  form button:hover {
      background: #FF5E5E;
  }
  form input,
  form button {
      font-family: 'Quicksand', sans-serif;
      height: 3rem;
      outline: none;
  }
</style>
