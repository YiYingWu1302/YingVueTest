<template>
    <div class="hi">
        <fieldset>
            <legend>
                Creat New Person
            </legend>
            <div>
                <label>Name:</label>
                <b-form-input type="text" class="h-50 w-25 mb-1" v-model=newPerson.name placeholder="Ying"></b-form-input>
            </div>
            <div>
                <label>Email:</label>
                <b-form-input type="text" class="h-50 w-25 mb-1" v-model=newPerson.email></b-form-input>
            </div>
            <div>
                <label>Sex:</label>
                <select>
                    <option value="Male">Male</option>
                    <option value="Female">Female</option>
                </select>
            </div>
            <b-btn size="sm" class="mb-2" @click="greet">Test</b-btn><br>
            <b-btn size="sm" class="mb-2" @click="createPerson">Create</b-btn><br>
            <b-btn size="sm" @click="updatePerson">Update</b-btn>
            <b-form-input type="number" class="h-50 w-25 mb-1" v-model=updateIndex></b-form-input>
        </fieldset>
        <div>
            <b-table striped hover :items="people" :fields='fields'>
                <template v-slot:delete="data">
                    <b-btn size="sm" @click="deletePerson(data.item.id)">Delete</b-btn>
                </template>
            </b-table>
        </div>
        <b-btn size="sm" @click="getPersons">Get from DB</b-btn>
    </div>
</template>
<script>
const API_URL = 'https://yingweb.azurewebsites.net/printHello'
const CREATE_USER = 'https://yingweb.azurewebsites.net/users'
const GET_USERS = 'https://yingweb.azurewebsites.net/users'
const UPDATE_USER = 'https://yingweb.azurewebsites.net/users/'

export default {
  name: 'hi',
  data () {
    return {
      updateIndex: 1,
      newPerson: {
        name: '',
        email: ''
      },
      fields: {
        name: {
          label: 'Name'
        },
        email: {
          label: 'Email'
        },
        delete: {
          label: 'Delete'
        }
      },
      people: [{
        name: 'Jack',
        email: 'a@com.com'
      },
      {
        name: 'Ying',
        email: 'b@com.com'
      }]
    }
  },

  methods: {
    greet: function () {
      this.$ajax.get(API_URL).then((response) => {
        console.log(response)
        alert(response.data)
      }).catch((error) => {
        console.log(error)
      })
    },
    createPerson: function () {
      // this.people.push(this.newPerson)
      this.$ajax.post(CREATE_USER, this.newPerson).then((response) => {
        console.log(response)
        alert(response.data)
      }).catch((error) => {
        console.log(error)
      })
      this.newPerson = {name: '', email: '', sex: 'Male'}
    },
    deletePerson: function (index) {
      // this.people.splice(index, 1)
      this.$ajax.delete(UPDATE_USER + index).then(res => {
        this.getPersons()
      })
    },
    savePerson: function (person) {
      this.$ajax.post(API_URL, person).then((response) => {
        this.deletePerson(0)
      }).catch((error) => console.log(error))
    },
    getPersons: function () {
      this.$ajax.get(GET_USERS).then((response) => {
        console.log(response)
        var arr = response.data
        this.people = arr
      }).catch((error) => console.log(error))
    },
    updatePerson: function () {
      this.$ajax.put(UPDATE_USER + this.updateIndex, this.newPerson).then((res) => {
        this.getPersons()
      })
    }
  }
}
</script>

<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
input, select {
    display: inline-block
}
</style>
