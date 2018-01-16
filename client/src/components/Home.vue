<template>
  <div class="">
    
    <div class="row" >
      <div class="card bg-light mb-3 row-md-4" style="max-width: 20rem;">
        <div class="card-header">Welcome {{user.name}}</div>
        <div class="card-body">
          <p> My Point :  {{user.point}}</p>
          <div class="list-group">
            <button v-on:click="goToBoard" type="button" class="btn btn-info btn-lg">Go to Board</button> <br>
            <!-- Trigger the modal with a button -->
            <button type="button" class="btn btn-info btn-lg" data-toggle="modal" data-target="#myModal">Add Todo</button> <br>
          </div>
        </div>
      </div>
      <div class="row-md-7">
        <table v-if="myTodo.length > 0" border="1px" class="table table-hover">
          <thead>
            <tr class="table-info">
              <th scope="col">Status</th>
              <th scope="col">Todo</th>
              <th scope="col">Time Added</th>
              <th scope="col">Action</th>
            </tr>
          </thead>
          <tbody v-for="todo in myTodo">
              <tr>
                <td v-if="todo.isComplete == true">[x]</td>
                <td v-else>[ ]</td>
                <td>{{todo.name}}</td>
                <td>{{todo.timeAdd}}</td>
                <td>
                  <button v-on:click="complete(todo._id)" type="button" name="button">Complete</button> <br> <br>
                  <button v-on:click="uncomplete(todo._id )"type="button" name="button">Uncomplete</button> <br> <br>
                  <button v-on:click="remove(todo._id)"type="button" name="button">Delete</button> <br> <br>
                </td>
              </tr>
          </tbody>
        </table>
        <h4 v-else> you didn't have any todo </h4>
      </div>
    </div>


    <!-- Modal -->
    <div id="myModal" class="modal fade" role="dialog">
      <div class="modal-dialog">

        <!-- Modal content-->
        <div class="modal-content">
          <div class="modal-header">
            <button type="button" class="close" data-dismiss="modal">&times;</button>
            <h4 class="modal-title">Add Todo</h4>
          </div>
          <div class="modal-body">
            <form>
              <fieldset>
                <legend>Add Todo</legend>
                <div class="form-group">
                  <input v-model="newTodo" type="text" class="form-control" placeholder="Enter New Todo">
                </div>
                </fieldset>
                <button v-on:click="addTodo" type="submit" class="btn btn-primary">Add</button>
              </fieldset>
            </form>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data () {
    return {
      user: 'belom',
      myTodo: [],
      url: 'http://localhost:3000/api/'
    }
  },

  mounted () {
    this.fetchDataUser()
    this.fetchDataTodos()
  },

  methods: {
    goToBoard() {
      this.$router.push('/board')
    },
    showtodo () {
      console.log(this.myTodo);
    },
    fetchDataUser () {
      let self = this
      this.$http.get(this.url + 'users',
        {
          headers: {
            token: localStorage.getItem('token')
          }
        })
        .then((response) => {
          self.user = response.data.data
        }).catch((err) => {
          console.log(err)
        })
    },

    fetchDataTodos () {
      this.$http.get(this.url + `users/getTodo/`, {
        headers: {
          token: localStorage.getItem('token')
        }
      })
      .then((response) => {
        this.myTodo = response.data.data
      }).catch((err) => {
        console.log(err)
      })
    },
    addTodo () {
      this.$http.post(this.url + `users/addTodo`, {
        name: this.newTodo
      }, {
        headers: {
          token: localStorage.getItem('token')
        }
      })
      .then((response) => {
        console.log(response.data.data)
        this.myTodo.push(response.data.data)
      }).catch((err) => {
        console.log(err)
      })
    },
    complete (id) {
      this.$http.put(this.url + `users/completeTodo/${id}`)
      .then((response) => {
        this.myTodo.map((todo) => {
          if (todo._id === response.data.data._id) {
            todo.isComplete = true
          }

        })
      }).catch((err) => {
        console.log(err)
      })
    },
    uncomplete (id) {
      this.$http.put(this.url + `users/uncompleteTodo/${id}`)
      .then((response) => {
        this.myTodo.map((todo) => {
          if (todo._id === response.data.data._id) {
            todo.isComplete = false
          }

        })
      }).catch((err) => {
        console.log(err)
      })
    },
    remove (id) {
      this.$http.delete(this.url + `users/deleteTodo/${id}`)
      .then((response) => {
        this.myTodo.map((todo, idx) => {
          if (todo._id === response.data.dataDeleted._id) {
            this.myTodo.splice(idx, 1)
          }
        })
      }).catch((err) => {
        console.log(err)
      })
    }
  }
}
</script>

<style lang="css">
</style>
