<template>
  <tr class="hk-row">
    <td>
      <select @change="$emit('update-hook', {$event, field: 'type'})">
        <option v-for="(value, key) in types" :selected="value === hook.type">{{value}}</option>
      </select>
    </td>
    <td>
      <select @change="$emit('update-hook', {$event, field: 'action'})">
        <option :selected="hook.action === 'CREATE'">CREATE</option>
        <option :selected="hook.action === 'UPDATE'">UPDATE</option>
        <option :selected="hook.action === 'DELETE'">DELETE</option>
      </select>
    </td>
    <td>
      <select @change="$emit('update-hook', {$event, field: 'filter'})">
        <option
          v-for="(filter, index) in nestedArrayMerge(filters['default'], filters[hook.type])"
          v-if="filter"
          :selected="filter.alias === hook.filter"
        >{{filter.alias}}</option>
      </select>
    </td>
    <td>
      <input @change="$emit('update-hook', {$event, field: 'endpoint'})" type="url" :value="hook.endpoint">
    </td>
    <td>
      <select @change="$emit('update-hook', {$event, field: 'endpoint_filter'})">
        <option
          v-for="(endpoint_filter, index) in nestedArrayMerge(endpoint_filters['default'], endpoint_filters[hook.type])"
          v-if="endpoint_filter"
          :selected="endpoint_filter.alias === hook.endpoint_filter"
        >{{endpoint_filter.alias}}</option>
      </select>
    </td>
    <td>
      <select @change="$emit('update-hook', {$event, field: 'authmethod'})">
        <option :selected="hook.authmethod === 'Basic'">Basic</option>
        <option :selected="hook.authmethod === 'None'">None</option>
      </select>
    </td>
    <td>
      <input @change="$emit('update-hook', {$event, field: 'authtoken'})" type="text" :value="hook.authtoken" :disabled="hook.authmethod === 'None'">
    </td>
    <td>
      <select @change="$emit('update-hook', {$event, field: 'success_callback'})">
        <option
          v-for="(success_callback, index) in nestedArrayMerge(success_callbacks['default'], success_callbacks[hook.type])"
          v-if="success_callback"
          :selected="success_callback.alias === hook.success_callback"
        >{{success_callback.alias}}</option>
      </select>
    </td>
    <td>
      <button @click="$emit('remove-hook', {id: hook.id})">&times;</button>
    </td>
  </tr>
</template>

<script>
export default {
  props: ['hook'],
  data: function(){
    return {
      types: window.hooky_types,
      filters: window.hooky_filters,
      endpoint_filters: window.hooky_endpoint_filters,
      success_callbacks: window.hooky_success_callbacks
    }
  },
  methods: {
    nestedArrayMerge: (...items) =>{
      let merged = []
      for(let item of items){
        if(!item) continue
        merged.push(...item)
      }
      return merged
    }
  }
}
</script>

<style>
.hk-row input,
.hk-row select {
  width: 100%;
  margin: 0;
}
</style>
