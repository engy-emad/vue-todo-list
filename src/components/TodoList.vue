<template>
<!---------------------------- START MAIN -------------------------->

<!---------------------------- START TODOLIST ---------------------->

    <main id="todolist">

        <h1>Todo List  <span>{{ tomorrow()}}</span></h1>

        <div class="items" v-if="items.length">

            <ul>

                <li v-for="(item, index) in items" :key="index" :class="{'done' : item.done}">
                
                    <span class="label">{{item.title}}</span>

                    <div class="actions">

                        <button class="btn-picto" type="button" @click="changeItemStatus(index)">
                            <i aria-hidden="true" class="material-icons">{{item.done ? 'check_box' :
                                'check_box_outline_blank'}}</i>
                        </button>

                        <button class="btn-picto" type="button"
                                aria-label="Delete" title="Delete" @click="deleteItem(index)">
                            <i aria-hidden="true" class="material-icons">delete</i>
                        </button>

                    </div>

                </li>

            </ul>

        </div>

        <p class="emptylist" v-else>Your todo list is empty.</p>

        <form @submit.prevent="addItem">

            <label for="newitem">Add to the todo list</label>
            <input type="text" name="newitem" id="newitem" v-model="itemTitle">
            <button type="submit">ADD</button>
            <button class="btn-detete" type="button"
                     aria-label="Delete" title="Delete" @click="deleteAll(index)">
                            <i aria-hidden="true" class="material-icons">Clear</i>
            </button>
        </form>
        

    </main>
    <!----------------------------END MAIN -------------------------->
</template>

<script>

    const localKey = 'todos';

    export default {

        name: 'TodoList',
  props: {
    msg: String
  },
      
      data() {
            return {
                itemTitle: '',
                items: [],
            }
        },

        methods: {
              tomorrow() {
                  const d = new Date()
                  d.setDate(d.getDate() )
                   return d 
  },

            addItem() {
                if (!this.itemTitle) {
                    return;
                }

                this.items.push({
                    title: this.itemTitle,
                    done: false,
                });

                this.itemTitle = '';
            },

            deleteItem(index) {
                this.items.splice(index, 1);
            },
              deleteAll(index) {
                this.items.splice(index);
            },

            changeItemStatus(index) {
                const item = this.items[index];
                this.items[index].done = !item.done;
            }
        },

        mounted() {
            const items = localStorage.getItem(localKey) || '[]';
            this.items = JSON.parse(items);
        },

        watch: {
            items: {
                deep: true,
                handler(items) {
                    localStorage.setItem(localKey, JSON.stringify(items))

                }
            }
        }
    }
</script>

<style>

/* ================ STYLE GLOBAL ================*/
    * {
        margin: 0;
        padding: 0;
        outline: none;
        box-sizing: border-box;
    }

     html{
         font-size: 1.1rem;
     }

     body {
        background-image: linear-gradient(62deg, rgba(1, 95, 183, 0.9732216701223994) 13%, rgba(255, 122, 151, 0.5) 4%),linear-gradient(44deg, rgba(0, 43, 99, 0.07922090238615942) 39%, rgba(242, 140, 143, 0.5) 18%),linear-gradient(118deg, rgba(84, 202, 242, 0.03152997265339608) 40%, rgba(247, 155, 187, 0.5) 54%),linear-gradient(58deg, rgba(90, 90, 237, 0.16144443572260592) 83%, rgba(249, 156, 142, 0.5) 23%); background-blend-mode: normal,lighten,multiply,hard-light;
        font-family: 'Quicksand', sans-serif;
        height: 100vh;
        
    }

:root{
    --light-gray:#f7f1f1;
    --pink:  #10101d;
    --light-pink:#FF5E5E;
    --white:#fff;
    --light-black:rgba(100, 100, 100, .1);
}

/* ================ END GLOBAL ================*/

/* ================ STYLE MAIN ================*/

    @keyframes strikeitem {
        to {
            width: calc(100% + 1rem);
        }
    }

/* ================ STYLE TODOLIST ================*/

    #todolist {
        margin: 4rem auto;
        padding: 2rem 3rem 3rem;
        max-width: 500px;
        background: var(--pink);
        color:var(--white);
        border-radius: 10px;
         border: 2px solid #fff;
    }

    #todolist h1 {
        font-weight: normal;
        font-size: 2.6rem;
        letter-spacing: 0.05em;
        border-bottom: 1px solid var(--light-black);
    }

    #todolist h1 span {
        display: block;
        font-size: 0.8rem;
        line-height: 2;
        margin-left: 3px;
        margin-block: 0.2rem 0.8rem;
    }

    #todolist .emptylist {
        margin-top: 2.6rem;
        text-align: center;
        letter-spacing: .05em;
        font-style: italic;
        opacity: 0.8;

    }

    #todolist ul {
        margin-top: 2.6rem;
        list-style: none;
    }

    #todolist .todolist-move {
        transition: transform 1s;
    }

    #todolist li {
        display: flex;
        margin: 0 -3rem 4px;
        padding: 1.1rem 3rem;
        justify-content: space-between;
        align-items: center;
        background: rgba(255, 255, 255, 0.1);
    }

    #todolist .actions {
        flex-shrink: 0;
        padding-left: 0.7em;
    }

    #todolist .label {
        position: relative;
        transition: opacity .2s linear;
    }

    #todolist .done .label {
        opacity: .6;
    }

    #todolist .done .label:before {
        content: '';
        position: absolute;
        top: 50%;
        left: -.5rem;
        display: block;
        width: 0%;
        height: 1px;
        background: var(--white);
        animation: strikeitem .3s ease-out 0s forwards;
    }

    #todolist .btn-picto {
        border: none;
        background: none;
        -webkit-appearance: none;
        cursor: pointer;
        color: var(--white);
    }


   /* ================ STYLE FORM ================*/

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
         width: 50%;
        border: none;
        background: var(--light-gray);
        padding: 0 1.5em;
        font-size: initial;
    }

    form button {
        padding: 0 1.3rem;
        border: none;
        background:var(--pink);
        color: white;
        text-transform: uppercase;
        font-weight: bold;
        border: 1px solid rgba(255, 255, 255, .3);
        margin-left: 5px;
        cursor: pointer;
        transition: background .2s ease-out;
    }

    form button:hover {
        background:var(--light-pink);
    }

    form input,
    form button {
        margin-top: 1rem;
        height: 2.5rem;
    }
    
    /* ================ END MAIN ================*/
</style>