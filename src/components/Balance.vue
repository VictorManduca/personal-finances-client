<script>
import axios from 'axios'

export default {
  data() {
    return {
      income: [0],
      outcome: [0],
      result: null
    }
  },
  methods: {
    onEnter: async function (event) {
      const enterKeyCode = 13
      if (event.keyCode == enterKeyCode) {
        const incomeClassName = 'income'
        const outcomeClassName = 'outcome'
        const incomeInput = this.createInput(incomeClassName)
        const incomeLine = document.createElement('td')
        incomeLine.append(incomeInput)

        const outcomeInput = this.createInput(outcomeClassName)
        const outcomeLine = document.createElement('td')
        outcomeLine.append(outcomeInput)

        this.appendInputsOnTable(incomeLine, outcomeLine)
        this.income = this.updateValues(incomeClassName)
        this.outcome = this.updateValues(outcomeClassName)

        const response = await this.callApi()
        if (response.status == 200) {
          this.result = response.data.message
        }
      }
    },
    createInput(inputName) {
      const resultInput = document.createElement('input')
      resultInput.setAttribute('class', inputName)
      resultInput.setAttribute('type', 'number')
      resultInput.addEventListener('keyup', this.onEnter)

      return resultInput
    },
    appendInputsOnTable(firstInput, secondInput) {
      const newLineOnTable = document.createElement('tr')
      newLineOnTable.append(firstInput)
      newLineOnTable.append(secondInput)

      const content = document.getElementById('content')
      content.append(newLineOnTable)
    },
    updateValues(className) {
      const element = document.getElementsByClassName(className)
      let arr = []

      for (let i = 0; i < element.length; i++) {
        if (element[i].value == '') {
          arr.push(0)
        } else {
          arr.push(parseInt(element[i].value))
        }
      }

      return arr
    },
    callApi() {
      return axios.post('https://personal-finances-server.herokuapp.com/', {
        income: this.income,
        outcome: this.outcome
      })
    }
  }
}
</script>

<template>
  <p>Result: {{ this.result ? this.result : 'none yet' }}</p>

  <table id="content">
    <tr>
      <th>Income</th>
      <th>Outcome</th>
    </tr>
    <tr>
      <td><input v-on:keyup.enter="onEnter" type="number" class="income"></td>
      <td><input v-on:keyup.enter="onEnter" type="number" class="outcome"></td>
    </tr>
  </table>
</template>

<style>
</style>