<template>
  <q-split-layout :title="$route.name">
    <template v-slot:side>
      <q-list>
        <q-item-label header>
          <div class="row justify-between items-center">
            <div>{{tasks.length}} Tasks</div>
            <opus-btn-dropdown-select
              v-model="selectedFilter"
              :options="filters"
              class="col-auto" />
          </div>
        </q-item-label>
      </q-list>
      <task-list
        v-if="tasks.length"
        :tasks="tasks"
        content-classes="bg-none"
        @click="showTaskDetailsDialog">
        <template v-slot:title="{ task }">
          <div class="text-subtitle2 ellipsis">
            {{ task.name }}
          </div>
        </template>
        <template v-slot:actions="{ task }">
          <opus-more-actions-btn
            :target="task"
            :actions="actions" />
        </template>
      </task-list>
      <opus-img-caption
        v-else
        width="170px"
        src="empty.svg"
        caption="Nothing to see here..."
      />
      <q-page-sticky
        style="z-index: 2001"
        :offset="[18, -18]"
        position="bottom-right">
        <opus-fab
          size="md"
          :show="!hasOpenDialog"
          @click="showNewTaskDialog" />
      </q-page-sticky>
    </template>
    <q-page class="bg-grey-1 fit">
      <div class="window-height flex flex-center">
        <pomodoro />
      </div>
      <div class="absolute-bottom-right q-ma-lg" style="width: 50%">
        <active-task
          :task="activeTask"
          @click="showTaskDetailsDialog(activeTask)"
          @complete="markComplete(activeTask)"
          @close="clearActiveTask()"
        />
      </div>
    </q-page>
  </q-split-layout>
</template>
<script>
import UseTasks from 'pages/mixin-tasks'

import OpusFab from 'lib/commons/OpusFab'
import OpusBtnDropdownSelect from 'lib/commons/OpusBtnDropdownSelect'
import OpusImgCaption from 'lib/commons/OpusImgCaption'
import OpusMoreActionsBtn from 'lib/commons/OpusMoreActionsMenuBtn'

import TaskList from 'components/tasks/TaskList'
import ActiveTask from 'components/tasks/ActiveTask'
import Pomodoro from 'lib/commons/Pomodoro'

export default {
  name: 'TasksPage',
  mixins: [UseTasks],
  components: {
    TaskList,
    ActiveTask,
    Pomodoro,
    OpusFab,
    OpusImgCaption,
    OpusMoreActionsBtn,
    OpusBtnDropdownSelect
  },
  data () {
    return {
      selectedFilter: 'recent',
      filters: [
        { label: 'Recent', value: 'recent' },
        { label: 'Ongoing', value: 'ongoing' },
        { label: 'To do', value: 'todo' },
        { label: 'Completed', value: 'completed' }
      ]
    }
  }
}
</script>
