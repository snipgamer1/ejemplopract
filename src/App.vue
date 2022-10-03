<template>
  <div class="container">
    <div class="row">
      <div class="col-xs-12">
        <h1 class="jumbotron">VueJs</h1>
      </div>
       <table class="table table-striped-columns">
          <thead>
            <tr>
              <th style="width:40px;">¿Cancelado?</th>
              <th>Descripción</th>
              <th style="width:200px;">Monto</th>
              <th style="width:100px;">¿Cancelado?</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td></td>
              <td > 
                <input type="text" class="from-control" v-model="newEntry.name" />
              </td>
              <td> 
                <input type="text" class="from-control" v-model="newEntry.amount" />
              </td>
              <td> 
                <button @click="add()" type="button" class="btn btn-success btn-block">Agregar</button>
              </td>
              
            </tr>

            <tr  v-if="items.length === 0">
              <td colspan="4" class="text-center">No se han encontrado pagos pendientes !</td>
            </tr>
            <tr  v-for="item, index in items">
              <td>
                <button @click="remove(index)" type="button" class="btn btn-danger btn-xs">
                  <i class="glyphicon glyphicon-trash"></i>
                </button> 
              </td>
              <td class="text-end"> {{ item.name}} </td>
              <td class="text-end"> {{ item.amount.toFixed(2) }} </td>
              <td class="text-center" :title="item.paid  ? 'Si' : 'No' "> 
                <button @click="changeToPaid(item)" class="btn btn-default btn-sm" v-bind:class="{'btn-success': item.paid} ">
                    <i v-if="item.paid" class="glyphicon glyphicon-ok"></i>
                    <i v-if="item.paid" class="glyphicon glyphicon-remove"></i>
                </button>
              </td>
            </tr>
          </tbody>
          <tfoot>
            <tr>
              <td></td>
              <td class="text-end">Por pagar</td>
              <td class="text-end">{{totalAmount(0)}}</td>
              <td></td>
            </tr>
            <tr>
              <td></td>
              <td class="text-end">Pagado</td>
              <td class="text-end"> {{totalAmount(1)}}</td>
              <td></td>
            </tr>
            <tr>
              <td></td>
              <td class="text-end">Total</td>
              <td class="text-end">{{totalAmount(2)}}</td>
              <td></td>
            </tr>
          </tfoot>
       </table>
    </div>
  </div>
</template>

<script>
  export default {
    data: () => ({
      newEntry:{
        name: '',
        amount: 0,
      },
      message: 'Hola Profesor.',
      items: [
        {name: 'Servicios', amount: 200, paid: false},
        {name: 'Hosting de Ricardo', amount: 90, paid: true },
      ],
    }),

    methods: {
      remove: function(index){
        this.items.splice(index, 1);
      },
      add(){
       this.items.push({
          name: this.newEntry.name,
          amount: parseFloat(this.newEntry.amount),
          paid: false
       });

       this.newEntry.name = '';
       this.newEntry.amount = 0;
       this.newEntry.name = '';
      },

      changeToPaid(item){
        item.paid = !(item.paid);
      },

      totalAmount(t){
        var total = this.items.reduce(function(a,b){
         switch(t){
            case 0: return a + (!b.paid ? b.amount : 0);
            case 1: return a + (b.paid ? b.amount : 0);
            case 2: return a + b.amount;
          
         }
        }, 0);
        return total.toFixed(2);
      }
    },
  }
</script>
