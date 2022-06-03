<template>
  <form>
    <div v-if="isEdit" class="form-group">
      <label for="Id">ID</label>
      <input
        type="text"
        class="form-control"
        id="formGroupExampleInput"
        v-model="user.id"
        disabled
      />
    </div>
    <div class="form-group">
      <label for="photo">Name</label>
      <input
        type="text"
        class="form-control"
        id="formGroupExampleInput2"
        placeholder="Enter Name"
        v-model="user.name"
      />
    </div>
    <div class="form-group">
      <label for="photo">Role</label>
      <input
        type="text"
        class="form-control"
        id="formGroupExampleInput2"
        placeholder="Enter Role"
        v-model="user.role"
      />
    </div>
    <div class="form-group">
      <label for="photo">Role 2</label>
      <input
        type="text"
        class="form-control"
        id="formGroupExampleInput2"
        placeholder="Enter Role 2"
        v-model="user.role"
      />
    </div>
    <div
      class="btn btn-primary"
      v-if="!isEdit"
      @click="createUsers()"
      data-dismiss="modal"
    >
      Add
    </div>
    <div
      class="btn btn-primary"
      v-if="isEdit"
      @click="updateUsers()"
      data-dismiss="modal"
    >
      Update
    </div>
  </form>
</template>

<script>
export default {
  name: 'UsersModal',

  props: {
    isEdit: {
      type: Boolean
    },

    userDetail: {
      type: Object,
      default: () => {},
    }
  },

  data() {
    return {
      user: {}
    }
  },

  watch: {
    userDetail() {
      if (this.userDetail) {
        this.user = Object.assign({}, this.userDetail);
      } else {
        this.user = {}
      }
    }
  },

  methods: {
    createUsers() {
      const reqPrams = {
        name: this.user.name,
        role: this.user.role,
      }
      this.$emit('addUser', reqPrams);
    },

    updateUsers() {
      const reqPrams = {
        id: this.user.id,
        name: this.user.name,
        role: this.user.role,
      }
      this.$emit('editUser', reqPrams);
    }
  }
}
</script>