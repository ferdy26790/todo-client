<template>
  <div class="container">
    <h1>Board Todo</h1>
    <table  border="1px" class="table table-hover">
      <thead>
        <tr class="table-info">
          <th scope="col">Request</th>
          <th scope="col">Todo</th>
          <th scope="col">point</th>
          <th scope="col">Action</th>
        </tr>
      </thead>
      <tbody v-for="board in todoBoard">
          <tr>
            <td>{{board.userPost}}</td>
            <td>{{board.name}}</td>
            <td>{{board.point}}</td>
            <td>
              <div v-if="board.userGet">
                <p>taken</p>
              </div>
              <div v-else>
                <button v-on:click="complete(board._id)" type="button" name="button">Take Request</button>
              </div>
            </td>
          </tr>
      </tbody>
    </table>
    <br> <br>
    <h2>My Board</h2>
    <table v-if="myBoard.length > 0" border="1px" class="table table-hover">
      <thead>
        <tr class="table-info">
          <th scope="col">user</th>
          <th scope="col">Todo</th>
          <th scope="col">point</th>
          <th scope="col">Action</th>
        </tr>
      </thead>
      <tbody v-for="board in myBoard">
          <tr>
            <td>{{board.userPost}}</td>
            <td>{{board.name}}</td>
            <td>{{board.point}}</td>
            <td>
              <div v-if="board.isComplete === false">
                <button v-on:click="complete(board._id)" type="button" name="button">Complete</button>
              </div>
              <div v-else>
                completed
              </div>
              <button v-on:click="remove(board._id)"type="button" name="button">Delete</button> <br> <br>
            </td>
          </tr>
      </tbody>
    </table>
    <h4 v-else> you didn't have any Boards todo </h4>
  </div>

</template>

<script>
export default {
  name: 'BoardTodo',
  data () {
    return {
      url: 'http://localhost:3000/api/',
      todoBoard: [],
      myBoard: []
    }
  },
  created () {
    this.fetchDataBoard()
    this.fetchMyBoard()
  },
  methods: {
    fetchDataBoard () {
      this.$http.get(this.url + `users/boards`)
      .then((response) => {
        this.todoBoard = response.data.data
        //console.log(this.todoBoard)
      }).catch((err) => {
        console.log(err)
      })
    },
    fetchMyBoard () {
      this.$http.get(this.url + `users/myboard`, {
        headers: {
          token: localStorage.getItem('token')
        }
      })
      .then((response) => {
        this.myBoard = response.data.data
      }).catch((err) => {
        console.log(err)
      })
    },
    complete (id) {
      let self = this
      this.$http.put(this.url + `users/completeBoard/${id}`, {}, {
        headers: {
          token: localStorage.getItem('token')
        }
      })
      .then((response) => {
        self.myBoard.forEach((board) => {
          if(board._id === response.data.data.id) {
            board.isComplete = true
          }
        })
      }).catch((err) => {
        console.log(err);
      })
    }
  }
}
</script>

<style lang="css">
</style>
