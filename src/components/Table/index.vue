<template>
  <div
    v-loading="loading"
    class="table"
  >
    <div class="table-body">
      <el-table
        v-bind="$attrs"
        :data="data"
        stripe
      >
        <template v-for="column in columns">
          <slot
            v-if="column.slot && !column.hidden"
            :name="column.slot"
          />
          <el-table-column
            v-if="column.buttons && !column.hidden"
            :key="column.label"
            v-bind="column"
          >
            <template v-slot="{row}">
              <el-button
                v-for="button in column.buttons.filter(button => (!button.ifRender || button.ifRender(row)))"
                :key="button.label"
                v-bind="button"
                @click="button.click(row)"
              >
                {{ button.label || row[button.prop] }}
              </el-button>
            </template>
          </el-table-column>
          <el-table-column
            v-else-if="!column.hidden"
            :key="column.label"
            v-bind="column"
            :hidden-overflow-tooltip="true"
          />
        </template>
      </el-table>
    </div>
    <div
      v-if="pagination"
      class="table-pagination"
    >
      <el-pagination
        :layout="layout"
        v-bind="pagination"
        @current-change="currentChange"
        @size-change="sizeChange"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: 'Table',
  props: {
    loading: {
      type: Boolean,
      default: false
    },
    data: {
      type: Array,
      required: true
    },
    columns: {
      type: Array,
      required: true
    },
    pagination: {
      type: Object,
      default: null
    },
    layout: {
      type: String,
      default: 'total, sizes, prev, pager, next, jumper'
    }
  },
  methods: {
    currentChange (val) {
      this.$emit('current-change', val)
    },
    sizeChange (val) {
      this.$emit('size-change', val)
    }
  }
}
</script>
