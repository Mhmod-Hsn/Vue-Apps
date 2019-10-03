<template>
    <div id="todo">
        <h3>This is My Todo List (my first app)</h3>
        <p>{{completed}}</p>
        <input @keyup.enter="addItem"
               placeholder="Write what to do then click enter"
               type="text"
               v-model="todoItem">

        <vs-list>
            <draggable v-model="todos">

                <vs-list-item
                        :class="{todoDone:todo.done}"
                        :key="index"
                        :subtitle="todo.date"
                        :title="todo.text"
                        class="text-left"
                        v-for="(todo, index) in todos">

                    <span @click="removeItem(index)"
                          class="pointer handle">
                        <vs-chip
                                color="danger">x</vs-chip>

                    </span>

                    <vs-switch color="primary"
                               v-model="todo.done"/>


                </vs-list-item>
            </draggable>

        </vs-list>

    </div>
</template>

<script>
    import draggable from 'vuedraggable'


    /* eslint-disable */
    export default {
        name: "Todo",
        components: {
            draggable,
        },
        data() {
            return {
                todos: [
                    {
                        text: 'Something to do',
                        date: '3/10/2019 @ 20:5:7',
                        done: false
                    },
                    {
                        text: 'Something to do',
                        date: '3/10/2019 @ 20:5:7',
                        done: false
                    },
                    {
                        text: 'Something to do',
                        date: '3/10/2019 @ 20:5:7',
                        done: true
                    },
                    {
                        text: 'Something to do',
                        date: '3/10/2019 @ 20:5:7',
                        done: true
                    }
                ],
                todoItem: '',
            }
        },
        methods: {
            addItem() {
                let currentdate = new Date();
                let datetime = currentdate.getDate() + "/"
                    + (currentdate.getMonth() + 1) + "/"
                    + currentdate.getFullYear() + " @ "
                    + currentdate.getHours() + ":"
                    + currentdate.getMinutes() + ":"
                    + currentdate.getSeconds();

                if (this.todoItem !== '') {
                    this.todos.push({
                        text: this.todoItem,
                        date: datetime,
                        done: false
                    });

                    this.$vs.notify({
                        title: 'Todo has been Added',
                        text: this.todoItem,
                        color: 'success'
                    });

                    this.todoItem = ''
                }
            },
            removeItem(index) {
                console.log(index);
                this.$vs.notify({
                    title: 'Todo has been Removed',
                    text: this.todos[index].text,
                    color: 'danger'
                });

                this.todos.splice(index, 1)
            }

        }
    }
</script>

<style lang="scss">
    .vs-notifications {
        font-family: "Avenir", Helvetica, Arial, sans-serif;
    }

    .pointer {
        cursor: pointer;
    }

    .vs-list--item {
        transition: all 0.3s ease-in-out;
        position: relative;

        .con-vs-chip {
            position: absolute !important;
            left: -30px;
            top: 12px;
            opacity: 0;
            transition: all 0.3s ease-in;

        }

        .list-titles {
            transition: all 0.3s ease-in;
        }

        &.todoDone {
            opacity: 0.5;

            .list-titles {
                text-decoration: line-through;
            }
        }
    }

    .vs-list--item:hover {

        .list-titles {
            margin-left: 30px;
        }

        .con-vs-chip {
            left: 0;
            top: 12px;
            opacity: 1;
        }
    }


    #todo {
        width: 600px;
        max-width: 100%;
        margin: auto;
        padding: 20px;

        input {
            display: block;
            width: 100%;
            padding: 7px 10px;
            font-size: 14px;
            -webkit-border-radius: 3px;
            -moz-border-radius: 3px;
            border-radius: 3px;
            border-style: groove;
        }

        .text-left {
            text-align: left;
        }

        span.remove {
            background-color: #41b883;
            color: #fff;
            width: 20px;
            height: 20px;
            text-align: center;
            border-radius: 50%;
            line-height: 20px;
            font-size: 12px;
            margin: 0;
            cursor: pointer;
            float: right;
        }
    }
</style>