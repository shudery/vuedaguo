<template>
    <div id="dialogue">
      <div id="header">
        <mu-appbar :title="user.name" :zDepth="0">
            <mu-icon-button icon="arrow_back" slot="left"/>
        </mu-appbar>
      </div>
      <div id="content">
        <div class="patch-1"></div>
        <dialogue :userData="userData" class="dialogue"
                    name="dialogue"
                    @scrollC="scrollC"
                    ></dialogue>
        <div class="patch-2"></div>
        <!--锚点-->
        <a name="1" href="#1" ref="end"
           style="height:0;color:rgba(0,0,0,0)">.</a>
      </div>
      <div id="footer">
          <input type="text" name="" class="input" value=""
                 v-model="todo" @keyup.enter="addTodo">
          <button type="button" name="button" @click="addTodo">发送</button>
        </div>
    </div>
</template>

<script>
import { mapState, mapActions, mapMutations } from "vuex";
import dialogue from "./Dialogue";

export default {
  name: "ChatBot",
  components: { dialogue },
  data() {
    return {
      todo: ""
    };
  },
  computed: {
    ...mapState({
      user: state => state.chatbot.data.user,
      bot: state => state.chatbot.data.bot
    }),
    userData() {
      return {
        user: this.user,
        bot: this.bot
      };
      console.log("2");
    }
  },
  methods: {
    // Dispatching Actions
    // ...mapActions(["getAllData"]),
    completed(index) {
      this.todos[index].isCompleted = !this.todos[index].isCompleted;
    },
    addTodo() {
      if (this.todo.length) {
        this.$store.dispatch("sendValue", {
          message: this.todo,
          id: this.userData.bot.id
        });
      } else {
        console.log("不能为空");
      }
      this.todo = "";
    },
    // 监听子组件事件
    scrollC() {
      // 取巧的方法，每次组件更新后模拟点击，破坏性的修改哈希值，但是简便（此处可以修改为正常控制滚动条）
      this.$refs.end.click();
    }
  }
};
</script>

<style lang="less" scoped>
#todoList {
  margin: auto;
  /* background-color: #e2e2e2; */
  /* width: 50%; */
  /* max-width: 350px; */
}

#header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 40px;
  border-bottom: 1px solid #e2e2e2;
  padding: 15px 0;
  text-align: center;
}

#content {
  padding: 0;
  position: relative;
}

.patch-1 {
  height: 70px;
}

.input {
  border-color: #e2e2e2;
  /* height: 20px; */
  width: 60%;
  /* min-width: 200px; */
  margin: 15px 0;
  line-height: 30px;
}

#footer {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 70px;
  text-align: center;
  background: #fff;
  border-top: 1px solid #e2e2e2;
}
</style>

