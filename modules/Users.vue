<template>
  <div class="container crud-users" style="margin-top: 50px">
    <div class="row flex-lg-nowrap">
      <div class="col">
        <div class="row flex-lg-nowrap">
          <div class="col mb-3">
            <div class="e-panel card">
              <div class="card-body">
                <div class="card-title">
                  <h6 class="mr-2">
                    <span>Users</span
                    ><small class="px-1">Be a wise leader</small>
                  </h6>
                </div>
                <div class="e-table">
                  <div class="table-responsive table-lg mt-3">
                    <table class="table table-bordered">
                      <thead>
                        <tr>
                          <th class="align-top">ID</th>
                          <th>Name</th>
                          <th class="max-width">Role</th>
                          <th class="sortable">Role</th>
                          <th>Actions</th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr v-for="item in users" :key="item.id">
                          <td class="align-middle">{{ item.id }}</td>
                          <td class="align-middle">{{ item.name }}</td>
                          <td class="text-nowrap align-middle">
                            {{ item.role }}
                          </td>
                          <td class="text-nowrap align-middle">
                            <span>{{ item.role }}</span>
                          </td>
                          <td class="align-middle">
                            <div class="btn-group align-top">
                              <button
                                class="btn btn-sm btn-outline-secondary badge"
                                type="button"
                                data-toggle="modal"
                                data-target="#myModal"
                                @click="editClick(item)"
                              >
                                Edit
                              </button>
                              <button
                                class="btn btn-sm btn-outline-secondary badge"
                                type="button"
                                @click="remove(item.id)"
                              >
                                <i class="fa fa-trash"></i>
                              </button>
                            </div>
                          </td>
                        </tr>
                      </tbody>
                    </table>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="col-12 col-lg-3 mb-3">
            <div class="card">
              <div class="card-body">
                <div class="text-center px-xl-3">
                  <button
                    class="btn btn-success btn-block"
                    type="button"
                    data-toggle="modal"
                    data-target="#myModal"
                    @click="addClick()"
                  >
                    New User
                  </button>
                </div>
                <hr class="my-3" />
              </div>
            </div>
          </div>
        </div>

        <!-- User Form Modal -->
        <div class="modal fade" id="myModal">
          <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
              <!-- Modal Header -->
              <div class="modal-header">
                <h4 class="modal-title">{{ modalTitle }}</h4>
                <button type="button" class="close" data-dismiss="modal">
                  &times;
                </button>
              </div>

              <!-- Modal body -->
              <div class="modal-body">
                <UsersModal :is-edit="isEdit" :user-detail="userDetail" @addUser="getDataUser" @editUser="getDataUser" />
              </div>

              <!-- Modal footer -->
              <div class="modal-footer">
                <button
                  type="button"
                  class="btn btn-secondary"
                  data-dismiss="modal"
                >
                  Close
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import UsersModal from '../modules/UsersModal.vue'

export default {
  name: 'crud-user',
  data() {
    return {
      isEdit: false,
      users: [],
      modalTitle: '',
      userDetail: {},
      newUser: {},
    //   userDate: '',
    }
  },

  components: {
      UsersModal
  },

  mounted() {
    this.getUsers()
  },
  methods: {
    getUsers() {
      axios
        .get(
          'http://localhost:8080/api/user'
        )
        .then((data) => {
          this.users = data.data
        })
    },

    addClick() {
      this.isEdit = false
      this.modalTitle = 'Add Users'
      this.userDetail = {}
    },

    editClick(item) {
      this.isEdit = true
      this.modalTitle = 'Edit Users'
      this.userDetail = item;
    },

    getDataUser(val) {
      this.newUser = val;
      if (this.isEdit) {
        this.updateUsers(this.newUser.id);
      } else {
        this.createUsers();
      }
    },

    createUsers() {
      axios
        .post(
          'http://localhost:8080/api/user/add',
          this.newUser
        )
        .then(() => {
          this.getUsers()
        })
    },

    updateUsers(id) {
      axios
        .put(
          'http://localhost:8080/api/user/update/?id=' +
            id,
          {
            name: this.newUser.name,
            role: this.newUser.role,
            // Date: this.userDate,
          }
        )
        .then(() => {
          this.getUsers()
        })
    },

    remove(index) {
      if (confirm('Do you wanna delete this user?')) {
        axios
          .delete(
            'http://localhost:8080/api/user/delete/' +
              index
          )
          .then(() => {
            this.getUsers()
          })
      }
    },
  },
}
</script>

