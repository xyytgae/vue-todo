<template>
  <div>
    <table border="3">
      <tr>
        <th>ID</th>
        <th>タイトル</th>
        <th>期限</th>
        <th>
          ステータス
          <!-- <select v-model="statusSort">
            <option>全て</option>
            <option>未着手</option>
            <option>作業中</option>
            <option>完了</option>
          </select> -->
        </th>
      </tr>
      <tr v-for="(todo, index) in todos" v-bind:key="todo.id">
        <td>{{ todo.id }}</td>
        <td width="200px">
          <div v-show="edited">{{ todo.title }}</div>
          <div>
            <input type="text" v-model="todo.title" v-show="editing" />
          </div>

          <button @click="edit">{{ buttonText }}</button>
        </td>
        <td>
          <el-date-picker type="date" v-model="todo.deadline"></el-date-picker>
        </td>
        <td width="360px">
          <form>
            <input
              type="radio"
              name="statusButton"
              id="yet"
              value="未着手"
              @click="statusChange('未着手', index)"
              checked
            />
            <label for="yet">未着手</label>
            <input
              type="radio"
              name="statusButton"
              id="working"
              value="作業中"
              @click="statusChange('作業中', index)"
            />
            <label for="working">作業中</label>
            <input
              type="radio"
              name="statusButton"
              id="done"
              value="完了"
              @click="statusChange('完了', index)"
            />
            <label for="done">完了</label>
            <span class="status-class">{{ todo.status }}</span>
            <button @click="deleteButton(index)">削除</button>
          </form>
        </td>
      </tr>
    </table>

    <br />
    <div class="RootApp">
      <el-form ref="form" label-width="70px">
        <el-form-item label="タイトル">
          <el-input
            type="text"
            v-model="addTitle"
            placeholder="todoのタイトル"
            required
          ></el-input>
          {{ msg }}
        </el-form-item>
        <el-form-item label="期限">
          <el-date-picker type="date" v-model="form.day"></el-date-picker>
        </el-form-item>
      </el-form>
    </div>

    <button @click="addButton">追加</button>
  </div>
</template>

<script>
// import Element from 'element-ui';
export default {
  data() {
    return {
      edited: true,
      editing: false,
      addTitle: '',
      msg: '',
      arrayNumber: 0,
      form: {
        day: new Date(),
      },
      todos: [],
      statusSort: '',
      // yetChecked: true,
      // workingChecked: false,
      // doneChecked: false,
    };
  },
  methods: {
    addButton() {
      if (this.addTitle === '') {
        this.msg = 'タイトルを入力してください';
        return;
      }

      this.todos.push({
        id: this.arrayNumber + 1,
        title: this.addTitle,
        deadline: this.form.day,
        status: '未着手',
      });

      this.addTitle = '';
      this.form.day = new Date();
      this.arrayNumber++;
    },
    statusChange(value, index) {
      this.$set(this.todos[index], 'status', value);
    },
    deleteButton(index) {
      this.todos.splice(index, 1);
    },
    edit() {
      for (let i = 0; i < this.todos.length; i++) {
        if (!this.todos[i].title) {
          alert('タイトルを入力してください');
          return;
        }
      }
      this.edited = this.edited ? false : true;
      this.editing = this.editing ? false : true;
    },
  },
  computed: {
    buttonText() {
      if (this.edited) {
        return '編集';
      } else {
        return '保存';
      }
    },
    // sortedTodos() {
    //   let array = [];
    //   if (this.statusSort === '未着手') {
    //     array = this.todos.filter(function(item) {
    //       if (item.status === '未着手') return true;
    //     });
    //     return array;
    //   } else if (this.statusSort === '作業中') {
    //     array = this.todos.filter(function(item) {
    //       if (item.status === '作業中') return true;
    //     });
    //     return array;
    //   } else if (this.statusSort === '完了') {
    //     array = this.todos.filter(function(item) {
    //       if (item.status === '完了') return true;
    //     });
    //     return array;
    //   } else {
    //     return this.todos;
    //   }
    // },
    // yetChecked() {
    //   if (this.statusSort === '未着手') {
    //     return true;
    //   }
    //   return false;
    // },
    // workingChecked() {
    //   if (this.statusSort === '作業中') {
    //     return true;
    //   }
    //   return false;
    // },
    // doneChecked() {
    //   if (this.statusSort === '完了') {
    //     return true;
    //   }
    //   return false;
    // },
  },
};
</script>

<style scoped>
.RootApp >>> .el-input {
  width: 15%;
}

.status-class {
  padding: 0.5em 1em;
  margin: 16px 16px;
  font-weight: bold;
  border: solid 3px #6091d3;
  border-radius: 10px;
}
</style>
