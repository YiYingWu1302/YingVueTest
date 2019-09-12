<template>
  <div class="create-card">
    <b-row>
      <b-col></b-col>
      <b-col cols="6">
        <h3>Project Members</h3>
        <h6> {{ description }} </h6>
        <!-- Form start-->
        <b-form @submit.prevent="createPerson">
          <!-- Name (with description text below) -->
          <b-form-group
            label="Name:"
            label-cols="2"
            label-for="input-name"
            description="Please type your English name with the format <first name>_<last name>."
          >
            <b-form-input id="input-name" v-model="formData.name" placeholder="Chu_Pika" required></b-form-input>
          </b-form-group>
          <!-- E-mail -->
          <b-form-group label="E-mail:" label-cols="2" label-for="input-email">
            <b-form-input id="input-email" v-model="formData.email" placeholder="Chu_Pika@asus.com"></b-form-input>
          </b-form-group>
          <!-- Sex (selector) -->
          <b-form-group label="Sex:" label-cols="2" label-for="input-sex">
            <b-form-select v-model="formData.sex">
              <option :value="null" disabled hidden>Select your gender</option>
              <option value="Female">Female</option>
              <option value="Male">Male</option>
            </b-form-select>
          </b-form-group>
          <!-- Age -->
          <b-form-group label="Age:" label-cols="2" label-for="input-age">
            <b-form-input id="input-age" v-model="formData.age" placeholder="25"></b-form-input>
          </b-form-group>
          <div class="row justify-content-center mb-2">
            <b-button class="pd-2" type="submit">Submit</b-button>
          </div>
        </b-form>
        <!-- Form end-->
      </b-col>
      <b-col></b-col>
    </b-row>
    <!-- <div class="row justify-content-center mb-2">
      <b-button @click="getPersons">Get from DB</b-button>
    </div> -->
    <b-table outlined hover :items="people" :fields="fields" align="center">
      <template v-slot:cell(action)="data">
        <b-button @click="deletePerson(data.item.id)" class="mr-2" variant="danger">Delete</b-button>
        <b-button @click="$bvModal.show('bv-modal-update'), selectedData = deepCopy(data.item)">Update</b-button>
      </template>
    </b-table>
    <b-modal id="bv-modal-update" title="Update member info" hide-footer centered>
      Update member's info <br>
      <b-form-input class="mb-1" v-model="selectedData.name" placeholder="name"></b-form-input>
      <b-form-input class="mb-1" v-model="selectedData.email" placeholder="email"></b-form-input>
      <b-form-input class="mb-1" v-model="selectedData.sex" placeholder="sex"></b-form-input>
      <b-form-input class="mb-1" v-model="selectedData.age" placeholder="age"></b-form-input>
      <b-button @click="$bvModal.hide('bv-modal-update'), updatePerson(selectedData.id)">Update</b-button>
    </b-modal>
  </div>
</template>

<script>
// const SERVER = '//localhost:4000' // "//" 延續這個網頁的 protocal (e.g., http). Notice: 不能只打 localhost..., 會變相對網址接在此網址的後面
const SERVER = 'https://yingweb.azurewebsites.net'
const CREATE_USER = SERVER + '/users'
const GET_USERS = SERVER + '/users'
const UPDATE_USER = SERVER + '/users/'
const DELETE_USER = SERVER + '/users/'

export default {
  name: 'CreateCard',
  props: {
    description: String
  },
  data () {
    return {
      selectedData: {},
      formData: {
        id: null,
        name: null,
        email: null,
        sex: null,
        age: null
      },
      fields: [
        {
          key: 'name',
          sortable: true
        },
        {
          key: 'email',
          label: 'E-mail'
        },
        'sex', 'age', 'action'
      ],
      people: []
    }
  },
  methods: {
    createPerson: function () {
      // this.people.push(this.newPerson)
      this.$ajax.post(CREATE_USER, this.formData).then((response) => {
        console.log(response)
        this.getPersons()
        this.resetForm()
      }).catch((error) => {
        console.log(error)
      })
      this.newPerson = { name: '', email: '', sex: 'Male' }
    },
    getPersons: function () {
      this.$ajax.get(GET_USERS).then((response) => {
        console.log(response)
        var arr = response.data
        this.people = arr
      }).catch((error) => console.log(error))
    },
    updatePerson: function (index) {
      this.$ajax.put(UPDATE_USER + index, this.selectedData).then((res) => {
        this.getPersons()
      })
    },
    deletePerson: function (index) {
      // this.people.splice(index, 1)
      this.$ajax.delete(DELETE_USER + index).then(res => {
        this.getPersons()
        // alert('success')
      })
    },
    resetForm: function () {
      this.formData = { id: null, name: null, email: null, sex: null, age: null }
    },
    deepCopy: function (data) {
      return Object.assign({}, data)
    }
  },
  beforeMount: function () {
    this.getPersons()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .form-control::placeholder{
    color:rgb(150, 150, 150);
  }
  .create-card{
    text-align: left;
    padding-left: 50px;
    padding-right: 50px;
  }
  h3{
    text-align: center;
    margin-top: 30px;
    margin-bottom: 10px;
  }
  h6{
    text-align: center;
    margin-bottom: 20px;
  }
  .icon{
    margin-right: 10px;
  }
  .icon i{
   cursor: pointer;
  }
</style>
