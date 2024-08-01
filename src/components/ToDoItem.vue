<template>
  <li>
    <div class="wrap" v-if="!isEditing">
      <p>{{ item.NAME }}</p>
      <p>{{ item.POSITION }}</p>
      <p>{{ item.EXPERIENCE }} лет</p>
    </div>

    <div v-else>
      <input v-model="editName" placeholder="Имя" @keyup.enter="saveEdit" />
      <input
        v-model="editPosition"
        placeholder="Должноссь"
        @keyup.enter="saveEdit"
      />
      <input
        v-model="editExperience"
        type="number"
        placeholder="Стаж"
        @keyup.enter="saveEdit"
      />
    </div>

    <button class="bottom" @click="isEditing ? saveEdit() : startEdit()">
      {{ isEditing ? 'Сохранить' : 'Изменить' }}
    </button>
    <button class="bottom" @click="isEditing ? cancelEdit() : $emit('delete')">
      {{ isEditing ? 'Отменить' : 'Удалить' }}
    </button>
  </li>
</template>

<script>
export default {
  props: {
    item: Object,
  },
  data() {
    return {
      isEditing: false,
      editName: this.item.name,
      editPosition: this.item.position,
      editExperience: this.item.experience,
      editId: this.item.id,
    };
  },
  methods: {
    startEdit() {
      this.isEditing = true;
    },
    async saveEdit() {
      if (
        this.editName.trim() &&
        this.editPosition.trim() &&
        this.editExperience >= 0
      ) {
        this.isEditing = false;
        this.$emit('edit', {
          id: +this.item.ID,
          name: this.editName,
          position: this.editPosition,
          experience: this.editExperience,
        });
      }
    },
    cancelEdit() {
      this.isEditing = false;
      this.editName = this.item.name;
      this.editPosition = this.item.position;
      this.editExperience = this.item.experience;
    },
  },
};
</script>

<style scoped>
.wrap {
  display: flex;
}

p {
  margin: 5px;
}

.wrap p,
.wrap .bottom {
  margin-right: 15px;
}
</style>
