<template>
    <div>
        <Message :msg="msg" :tipoStatus="tipoStatus" :titleMsg="titleMsg" v-show="msg"/>
        <div>
            <form id="form-pizza" @submit="createPizza">
                <div class="input-container">
                    <label for="nome">Nome do Cliente:</label>
                    <input type="text" name="nome" id="nome-cliente" v-model="nome_cliente" placeholder="Digite seu nome">
                </div>
                <div class="input-container">
                    <label for="tamanho">Escolha o tamanho:</label>
                    <select name="tamanho" id="tamanho" v-model="tamanho">
                        <option value="">Selecione o tamanho</option>
                        <option v-for="t in tamanhosAll" :key="t.id" :value="t.name">{{t.name}}</option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="massa">Escolha a massa:</label>
                    <select name="massa" id="massa" v-model="massa">
                        <option value="">Selecione a massa</option>
                        <option v-for="m in massasAll" :key="m.id" :value="m.name">{{m.name}}</option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="molho">Escolha o molho:</label>
                    <select name="molho" id="molho" v-model="molho">
                        <option value="">Selecione o molho</option>
                        <option v-for="m in molhosAll" :key="m.id" :value="m.name">{{m.name}}</option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="queijo">Escolha o queijo:</label>
                    <select name="queijo" id="queijo" v-model="queijo">
                        <option value="">Selecione o queijo</option>
                        <option v-for="q in queijosAll" :key="q.id" :value="q.name">{{q.name}}</option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="borda">Escolha a borda:</label>
                    <select name="borda" id="borda" v-model="borda">
                        <option value="">Selecione o queijo</option>
                        <option v-for="b in bordasAll" :key="b.id" :value="b.name">{{b.name}}</option>
                    </select>
                </div>
                <div class="input-container checkbox">
                    <p>Selecione os toppings</p>
                    <div class="all-checkbox">
                        <div class="checkbox_container" v-for="t in toppingsAll" :key="t.id">
                            <input type="checkbox" :name="t.name" :id="t.name" v-model="toppings" :value="t.name">
                            <label :for="t.name">{{t.name}}</label>                        
                        </div>
                    </div>
                </div>
                <div class="input-container checkbox">
                    <p>Selecione os extras</p>
                    <div class="all-checkbox">
                        <div class="checkbox_container" v-for="e in extrasAll" :key="e.id">
                            <input type="checkbox" :name="e.name" :id="e.name" v-model="extras" :value="e.name">
                            <label :for="e.name">{{e.name}}</label>                        
                        </div>
                    </div>
                </div>
                <div class="input-container">
                    <input type="submit" value="Criar minha pizza" class="submit-btn">
                </div>
            </form>
        </div>
    </div>
</template>
<script>
import Message from './Message.vue'

export default {
    name:'PizzaForm',
    components:{
        Message
    },
    data(){
        return{
            tamanhosAll: null,
            massasAll: null,
            molhosAll: null,
            queijosAll: null,
            toppingsAll: null,
            extrasAll: null,
            bordasAll: null,

            nome_cliente: null,
            tamanho: null,
            massa: null,
            molho: null,
            queijo: null,
            borda: null,
            toppings: [],
            extras: [],
            status: 'Solicitado',
            msg: null,
            titleMsg: null,
            tipoStatus:null
        }
    },
    methods:{
        async getIngredients(){
            const req = await fetch("https://makeyourburger-xxqo.onrender.com/pizzaPadrao");
            const data = await req.json();
            this.tamanhosAll = data.pizzaSizeOptions;
            this.massasAll = data.pizzaCrustOptions;
            this.molhosAll = data.pizzaSauceOptions;
            this.queijosAll = data.pizzaCheeseOptions;
            this.toppingsAll = data.pizzaIngredientsOptions;
            this.extrasAll = data.pizzaExtrasOptions;
            this.bordasAll = data.pizzaCrustFillingOptions;
        },
        async createPizza(e){
            e.preventDefault();
            
            const data= {
                nome: this.nome_cliente,
                tamanho: this.tamanho,
                massa: this.massa,
                molho: this.molho,
                queijo: this.queijo,
                borda: this.borda,
                toppings: Array.from(this.toppings),
                extras: Array.from(this.extras),
                status: 'Solicitado'
            }

            const dataJson = JSON.stringify(data);
            
            const req = await fetch("https://makeyourburger-xxqo.onrender.com/pizzasSolicitadas",{
                method: "POST",
                headers: { "Content-Type" : "application/Json" },
                body: dataJson
            });

            const res = await req.json();
            console.log(res);
            this.nome_cliente='';
            this.tamanho='';
            this.molho='';
            this.massa='';
            this.queijo='';
            this.borda='';
            this.toppings=[];
            this.extras=[];

            const req_pizza = await fetch("https://makeyourburger-xxqo.onrender.com/pizzasSolicitadas");
            const data_pizzas = await req_pizza.json();
            console.log(data_pizzas)
            this.titleMsg = `Seu pedido foi criado!`
            this.msg = `Seu pedido é o N°${data_pizzas.length}, obrigado por comprar conosco`
            this.tipoStatus = 'create'
            setTimeout(()=>this.msg="", 3000);  
        }
    },
    mounted(){
        this.getIngredients();
    }
}
</script>
<style>
    #form-pizza{
        max-width: 400px;
        margin: 50px auto;
    }
    .input-container{
        margin-top: 20px;
        margin-bottom: 30px;
        display: flex;
        justify-content: space-between;
    }
    .checkbox{
        display: flex;
        margin-top: 10px;
        flex-direction: column !important;
    }
    .checkbox_container>label{
        margin-left: 10px;
    }
    .checkbox_container{
        display: flex;
        justify-content: flex-start;
        align-items: center;
        width: 120px;
        margin:5px 20px;
    }
    .all-checkbox{
        display:flex;
        flex-wrap: wrap;
        justify-content: space-around;
    }
    select,
    input[type=text]{
        width: 60%;
        padding: 5px;
        border:2px solid #222;
        border-radius: 3px;
    }
    .submit-btn{
        width: 100%;
        padding:10px 0;
        transition: 0.5s;
        border: 2px solid #222;
        color: #FCBA07;
        font-weight: bold;
        cursor: pointer;
        font-size: 1.1rem;
        background: #222;    
    }
    .submit-btn:hover{
        background: transparent;
        color: #222;
    }
    .input-container>label,
    .input-container>p{
        font-weight: bold;
        border-left:4px solid #FCBA07;
        padding:5px;
    }
</style>
