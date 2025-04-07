<script>
export default {

  props:["onTagsChange"],
  name: "inputTag",
  data() {
    return {
      tags: [],
      currentValue: "",
    };
  },
  methods: {
    handleKeyDown(e) {
      if (e.key === "Backspace" && this.currentValue === "") {
        this.tags.pop();
        this.onTagsChange(this.tags);
      }
    },
    handleSubmit(e) {
      e.preventDefault();
      if (this.currentValue.trim() !== "") {
        const exist = this.tags.some((item) => item === this.currentValue);
        if (exist) {
          alert("Tag already exists");
          return;
        }
        if (this.currentValue.length > 10) {
          alert("Tag is too long");
          return;
        }
        if (this.currentValue.length < 3) {
          alert("Tag is too short");
          return;
        }

        this.tags.push(this.currentValue);
        this.currentValue = "";
        this.onTagsChange(this.tags);
        
      }
    },

    deleteTag(tag) {
      this.tags = this.tags.filter((item)=> item !== tag);
      this.onTagsChange(this.tags);

    },
  }
};
</script>

<template>

  <div class="inputTag">
    <div class="tags">
      <div class="tag" v-for="(tag,index) in tags" :key="index">
        {{ tag }} <button @click="deleteTag(tag)">X</button>
      </div>
    </div>
    <form @submit="handleSubmit">
      <input
        type="text"
        v-model="currentValue"
        @keydown="handleKeyDown"
        placeholder="Add a tag"
      />
    </form>
  </div>
</template>

<style scoped>
.inputTag {
  max-width: 400px;
  margin: 20px auto;
  font-family: Arial, sans-serif;
}

.tags {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-bottom: 15px;
}

.tag {
  background-color: #4caf50;
  color: white;
  padding: 5px 10px;
  border-radius: 15px;
  font-size: 14px;
  display: flex;
  align-items: center;
}

.tag button {
  background-color: transparent;
  border: none;
  color: white;
  font-weight: bold;
  margin-left: 8px;
  cursor: pointer;
}

.tag button:hover {
  color: #ff5252;
}

form {
  display: flex;
  gap: 10px;
}

input[type="text"] {
  flex: 1;
  padding: 8px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
}

input[type="text"]:focus {
  outline: none;
  border-color: #4caf50;
}

button {
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #388e3c;
}
</style>