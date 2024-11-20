//  listTodos.js
const db = require("./models/index");

const listTodo = async () => {
  try {
    await db.Todo.showList();
  } catch (error) {
    console.error(error);
  }
};
(async () => {
  await listTodo();
})();
My Todo-list

Overdue
24. [ ] Submit assignment 2022-07-10

Due Today
25. [x] Pay rent
28. [ ] Service vehicle

Due Later
26. [ ] File taxes 2022-07-14
27. [ ] Call Acme Corp. 2022-07-14
// models/todo.js
'use strict';
const {
  Model
} = require('sequelize');
module.exports = (sequelize, DataTypes) => {
  class Todo extends Model {
    /**
     * Helper method for defining associations.
     * This method is not a part of Sequelize lifecycle.
     * The `models/index` file will call this method automatically.
     */
    static async addTask(params) {
      return await Todo.create(params);
    }
    static async showList() {
      console.log("My Todo list \n");

      console.log("Overdue");
      // FILL IN HERE
      console.log("\n");

      console.log("Due Today");
      // FILL IN HERE
      console.log("\n");

      console.log("Due Later");
      // FILL IN HERE
    }

    static async overdue() {
      // FILL IN HERE TO RETURN OVERDUE ITEMS
    }

    static async dueToday() {
      // FILL IN HERE TO RETURN ITEMS DUE tODAY
    }

    static async dueLater() {
      // FILL IN HERE TO RETURN ITEMS DUE LATER
    }

    static async markAsComplete(id) {
      // FILL IN HERE TO MARK AN ITEM AS COMPLETE

    }

    displayableString() {
      let checkbox = this.completed ? "[x]" : "[ ]";
      return `${this.id}. ${checkbox} ${this.title} ${this.dueDate}`;
    }
  }
  Todo.init({
    title: DataTypes.STRING,
    dueDate: DataTypes.DATEONLY,
    completed: DataTypes.BOOLEAN
  }, {
    sequelize,
    modelName: 'Todo',
  });
  return Todo;
};
// addTodo.js
var argv = require('minimist')(process.argv.slice(2));
const db = require("./models/index")

const createTodo = async (params) => {
  try {
    await db.Todo.addTask(params);
  } catch (error) {
    console.error(error);
  }
};

const getJSDate = (days) => {
  if (!Number.isInteger(days)) {
    throw new Error("Need to pass an integer as days");
  }
  const today = new Date();
  const oneDay = 60 * 60 * 24 * 1000;
  return new Date(today.getTime() + days * oneDay)
}
(async () => {
  const { title, dueInDays } = argv;
  if (!title || dueInDays === undefined) {
    throw new Error("title and dueInDays are required. \nSample command: node addTodo.js --title=\"Buy milk\" --dueInDays=-2 ")
  }
  await createTodo({ title, dueDate: getJSDate(dueInDays), completed: false })
  await db.Todo.showList();
})();
// completeTodo.js
var argv = require('minimist')(process.argv.slice(2));
const db = require("./models/index");
const markAsComplete = async (id) => {
  try {
    await db.Todo.markAsComplete(id);
  } catch (error) {
    console.error(error);
  }
};

(async () => {
  const { id } = argv;
  if(!id) {
    throw new Error("Need to pass an id");
  }
  if(!Number.isInteger(id)) {
    throw new Error("The id needs to be an integer")
  }
  await markAsComplete(id);
  await db.Todo.showList();
})();
