<template>
        <div id="div-externa">
            <table>
                <thead id="burger-table-heading">
                    <tr>
                        <th class="order-id">#:</th>
                        <th>Cliente</th>
                        <th>Tamanho</th>
                        <th>Massa</th>
                        <th>Molho</th>
                        <th>Queijo</th>
                        <th>Borda</th>
                        <th>Toppings</th>
                        <th>Extras</th>
                        <th>Ações</th>
                    </tr>
                </thead>
                <tbody id="pizza-table-rows">
                    <tr class="pizza-table-row" v-for="d in data" :key="d.id" :class="d.status">
                        <td class="order-id">{{d.id}}</td>
                        <td>{{d.nome}}</td>
                        <td>{{d.tamanho}}</td>
                        <td>{{d.massa}}</td>
                        <td>{{d.molho}}</td>
                        <td>{{d.queijo}}</td>
                        <td>{{d.borda}}</td>
                        <td>
                            <ul>
                                <li v-for="(t, id) in d.toppings" :key="id">{{t}}</li>
                            </ul>
                        </td>
                        <td>
                            <ul>
                                <li v-for="(e, id) in d.extras" :key="id">{{e}}</li>
                            </ul>
                        </td>
                        <td class="actions-table">
                            <select name="status" class="status" @change="postAlterarStatusPedido($event, d.id)">
                                <option v-for="ds in dataStatus" :key="ds.id" :value="ds.tipo" :selected="d.status == ds.tipo">{{ds.tipo}}</option>
                            </select>
                            <button class="delete-btn" @click="deletePedido(d.id)">Cancelar</button>
                        </td>
                    </tr>
                </tbody>
            </table>
            <Message :msg="msg" :tipoStatus="tipoStatus" :titleMsg="titleMsg" v-show="msg"/>
        </div>
</template>
<script>
import Message from './Message.vue'
export default {
    name:'Dashboard',
    components:{
        Message
    },
    data(){
        return{
            data: null,
            dataStatus: null,
            msg:null,
            titleMsg:null,
            tipoStatus:'update',
            estado:null
        }
    },
    methods:{
        async getPedidosAndStatusJson(){
            const req = await fetch("https://makeyourburger-xxqo.onrender.com/pizzasSolicitadas");
            console.log(req)
            const data = await req.json();
            const reqStatus = await fetch("https://makeyourburger-xxqo.onrender.com/status");
            const dataStatus = await reqStatus.json();
            this.dataStatus = dataStatus;
            this.data = data;
        },
        async postAlterarStatusPedido(e, id){

            const option = e.target.value;
            const dataJson = JSON.stringify({status: option});

            const req = await fetch(`https://makeyourburger-xxqo.onrender.com/pizzasSolicitadas/${id}`,{
                method: "PATCH",
                headers: { "Content-Type" : "application/Json" },
                body: dataJson
            });

            const res = await req.json();
            console.log(res)
            this.getPedidosAndStatusJson();
            this.msg = `O pedido de ${res.nome} foi alterado para ${option}!`;
            this.titleMsg = `Status alterado`
            this.tipoStatus = 'update';
            setTimeout(()=>this.msg = '', 4000);
        },
        async deletePedido(id){
            const req = await fetch(`https://makeyourburger-xxqo.onrender.com/pizzasSolicitadas/${id}`,{
                method:"DELETE"
            });
            const res = await req.json();
            console.log(res);
            this.msg = `O pedido n°${id} foi deletado com sucesso!`;
            this.titleMsg = `Pedido excluido`
            this.tipoStatus = 'delete';
            this.getPedidosAndStatusJson();
            setTimeout(()=>this.msg = '', 4000);
        }
    },
    mounted(){
        this.getPedidosAndStatusJson();
    }
}
</script>
<style scoped> 
    .Finalizado{
        background: rgb(134, 214, 134) !important;
    }
    table{
        max-width: 75%;
        margin: 50px auto;
        border-collapse: collapse;
    }
    thead{
        background: #FCBA07;
        color: #222;
        border-bottom: 2px solid #222 !important;
    }
    th{
        font-weight: bold;
    }
    th,td{        
        padding: 10px 20px;
        margin: 0;
        text-align: center;
    }
    
    tr{
        border-bottom: 1px solid #222;
        font-size: 0.8rem;
    }
    .pizza-table-row:nth-child(even){
        background: #cccaca;
    }
    select{
        padding: 6px 12px ;
        margin-right: 15px;
    }
    button{
        padding: 5px 10px;
        background: #222;
        color: #FCBA07;
        transition: 0.5s;
        font-weight: bold;
        cursor: pointer;
    }
    button:hover{
        background: transparent;
        color: #222;
    }
    .actions-table{
        display: flex;
        flex-direction: row;
        justify-content: space-around;
    }
    /* Mobile devices */
    @media (max-width: 600px) {
        table {
            min-width: 600px;
        }
        #div-externa{
            overflow-x: scroll;
            max-width: 95%;
            margin: 0 0 0 5%;
        }

        th, td {
            padding: 5px 10px;
        }

        .actions-table {
            flex-direction: column;
            align-items: center;
        }
    }

    /* Tablets and small desktops */
    @media (min-width: 601px) and (max-width: 1024px) {
        #div-externa{
            overflow-x: scroll;
            max-width: 95%;
            margin: 0 0 0 5%;
        }
        table {
            min-width: 600px;
        }
        th, td {
            padding: 10px 20px;
        }
    }
    @media(max-width: 1500px){
        .actions-table {
            display: flex;
            flex-direction: column;
            align-items: flex-start;
        }
        select{
            min-width: 115px;
            margin-bottom: 5px;
        }
    }
    @media(prefers-color-scheme: dark){
        .pizza-table-row:nth-child(even){
            background: #cccaca;
            color: #222;
        }
    }
</style>