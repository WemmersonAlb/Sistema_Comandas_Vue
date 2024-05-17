<template>
        <div id="div-externa">
            <table>
                <thead id="burger-table-heading">
                    <tr>
                        <th class="order-id">#:</th>
                        <th>Cliente</th>
                        <th>Pão</th>
                        <th>Carne</th>
                        <th>Opcionais</th>
                        <th>Ações</th>
                    </tr>
                </thead>
                <tbody id="buger-table-rows">
                    <tr class="burger-table-row" v-for="d in data" :key="d.id">
                        <td class="order-id">{{d.id}}</td>
                        <td>{{d.nome}}</td>
                        <td>{{d.pao}}</td>
                        <td>{{d.carne}}</td>
                        <td>
                            <ul>
                                <li v-for="(op, id) in d.opcionais" :key="id">{{op}}</li>
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
            <Message :msg="msg" :tipoStatus="tipoStatus" v-show="msg"/>
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
            tipoStatus:'update'
        }
    },
    methods:{
        async getPedidosAndStatusJson(){
            const req = await fetch("https://makeyourburger-xxqo.onrender.com/burgers");
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

            const req = await fetch(`https://makeyourburger-xxqo.onrender.com/burgers/${id}`,{
                method: "PATCH",
                headers: { "Content-Type" : "application/Json" },
                body: dataJson
            });

            const res = await req.json();
            console.log(res)

            this.msg = `O status pedido de ${res.nome} foi alterado com sucesso!`;
            this.tipoStatus = 'update';
            setTimeout(()=>this.msg = '', 4000);
        },
        async deletePedido(id){
            const req = await fetch(`https://makeyourburger-xxqo.onrender.com/burgers/${id}`,{
                method:"DELETE"
            });
            const res = await req.json();
            console.log(res);
            this.msg = `O pedido n°${id} foi deletado!`;
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
    table{
        min-width: 75%;
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
        padding: 10px 50px;
        margin: 0;
        text-align: center;
    }
    .actions-table{
        display: flex;
        justify-content: space-around;
    }
    tr{
        border-bottom: 1px solid #222;
    }
    .burger-table-row:nth-child(even){
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
    /* Mobile devices */
    @media (max-width: 600px) {
        table {
            min-width: 600px;
            overflow-x:auto;
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
        table {
            min-width: 90%;
        }

        th, td {
            padding: 10px 20px;
        }

        .actions-table {
            flex-direction: row;
        }
    }

    /* Desktops and larger screens */
    @media (min-width: 1025px) {
        table {
            min-width: 75%;
        }

        th, td {
            padding: 10px 50px;
        }

        .actions-table {
            flex-direction: row;
        }
    }
</style>