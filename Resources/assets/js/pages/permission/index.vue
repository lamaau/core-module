<template>
  <v-inertable
    :allow-filter="false"
    :filters="inertable.filters"
    :data="inertable.data"
    :columns="inertable.columns"
  >
    <template #attributes>
      <button
        @click.prevent="create"
        type="button"
        class="btn-purple focus:ring-2 focus:ring-purple-600 focus:ring-offset-2"
      >
        New Permission
      </button>
    </template>
    <template #role="{ item }">
      <span
        class="rounded-md bg-purple-200 px-2 py-1 text-xs font-medium text-purple-700"
        >{{ item.role }}</span
      >
    </template>
    <template #created_at="{ item: { created_at } }">
      <span>{{ $helper.formatDate(created_at) }}</span>
    </template>
    <template #action="{ item: { id } }">
      <div class="flex space-x-2">
        <button
          class="rounded-md bg-yellow-400 p-2 focus:outline-none focus:ring-2 focus:ring-yellow-400 focus:ring-offset-2"
        >
          <v-icon name="PencilIcon" type="solid" class="h-3 w-3 text-white" />
        </button>
        <button
          @click.prevent="destroy(id)"
          type="button"
          class="rounded-md bg-red-500 p-2 focus:outline-none focus:ring-2 focus:ring-red-500 focus:ring-offset-2"
        >
          <v-icon name="TrashIcon" type="solid" class="h-3 w-3 text-white" />
        </button>
      </div>
    </template>
  </v-inertable>
</template>
<script>
export default {
  props: {
    inertable: Object,
  },
  methods: {
    create() {
      this.$modal.open({
        title: "New permission",
        component: require("./create.vue").default,
      });
    },
    destroy(id) {
      this.$modal.destroy({
        title: "Are you sure?",
        message: "This will permanently delete the permission from database",
        onAccept: () => {
          this.$inertia.delete(`/setting/role/permission/${id}`, {
            onSuccess: () => {
              this.$toast.success("Successfully delete permission");
              this.$modal.close();
            },
          });
        },
        onCancel: () => {
          this.$modal.close();
        },
      });
    },
  },
};
</script>
