<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header">Planilha</div>

                    <div class="card-body">
                        <table class="table table-bordered">
                            <thead>
                            <tr>
                                <th scope="col" style="width: 1%"></th>
                                <th scope="col" style="width: 3%; text-align: center">Célula</th>
                                <th scope="col" style="width: 10%">Obs*</th>
                            </tr>
                            </thead>
                            <tbody>
                            <tr>
                                <th scope="row" style="vertical-align: middle">1</th>
                                <td style="vertical-align: middle">
                                    <input type="text" class="form-control" v-model="table.cell"
                                           @keypress.enter="calcule">
                                </td>
                                <td>
                                    <ol>
                                        <li>
                                            Digite uma expressão aritmética. Ex.: <b>=2+1</b>
                                        </li>
                                        <li>Após inserir a expressão pressione: <b>Enter</b>
                                        </li>
                                    </ol>
                                </td>
                            </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "TableComponent",
    data: () => {
        return {
            table: {
                cell: null
            }
        }
    },
    methods: {
      calcule: function (){
          const arithmeticPattern = /[+*/-]?([0-9]+\.?[0-9]*)/gm;

          if(!this.isCellExpression() || !this.hasArithmeticOperation())
              return false

          let getValue = this.table.cell.match(arithmeticPattern);

          let getResult = eval(getValue.join(' '));

          if (!isFinite(getResult)) {
              this.table.cell = 0

              return false
          }

          this.table.cell = getResult
      },
      isCellExpression: function() {
          let isExpression = this.table.cell.match(/=(.*)/gm);

          return typeof isExpression !== undefined
            && isExpression !== null
            && (isExpression.length > 0);
      },
      hasArithmeticOperation: function () {
          let getOperator = this.table.cell.match(/[+*/-]/gm);

          return (typeof getOperator !== undefined && getOperator !== null && getOperator.length > 0)
      }
    },
}
</script>

<style scoped>

</style>
