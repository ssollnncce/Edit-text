<template>
    <div class="paper">
    <div class="container-button">
      <button class="action back" @click="goBack">
        <i class="fa fa-arrow-left"></i>
      </button>
      <button class="action go" @click="goForward">
        <i class="fa fa-arrow-right"></i>
      </button>
      <button class="action head" @click="makeHeading">
        <i class="fa fa-heading"></i>
      </button>
      <button class="action paragraph" @click="makeParagraph">
        <i class="fa fa-paragraph"></i>
      </button>
      <button class="action image" @click="insertImage">
        <i class="fa fa-image"></i>
      </button>
      <button class="copy" @click="copyHTML">Скопировать HTML</button>
    </div>
    <div class="word" contenteditable="true" @input="updateHistory" @click="hideText" ref="editableDiv">
      {{ isFirstClick ? 'Начните печатать здесь...' : '' }}
    </div>
  </div>
</template>

<script>
export default {
    name: "Main",
  data() {
    return {
      isFirstClick: true,
      history: [],
      currentIndex: -1,
    };
  },
  methods: {
    hideText() {
      if (this.isFirstClick) {
        this.$refs.editableDiv.innerText = "";
        this.isFirstClick = false;
      }
    },
    updateHistory() {
      // Обработчик события для элемента "word" при вводе текста
      this.history.splice(this.currentIndex + 1, this.history.length - this.currentIndex - 1);
      this.history.push(this.$refs.editableDiv.innerHTML);
      this.currentIndex = this.history.length - 1;
    },
    goBack() {
      if (this.currentIndex > 0) {
        this.currentIndex--;
        this.$refs.editableDiv.innerHTML = this.history[this.currentIndex];
      }
    },
    goForward() {
      if (this.currentIndex < this.history.length - 1) {
        this.currentIndex++;
        this.$refs.editableDiv.innerHTML = this.history[this.currentIndex];
      }
    },
    makeHeading() {
      const selection = window.getSelection();
      if (selection.toString() !== "") {
        const h1Element = document.createElement("h1");
        h1Element.textContent = selection.toString();
        const range = selection.getRangeAt(0);
        range.deleteContents();
        range.insertNode(h1Element);
      }
    },
    makeParagraph() {
      const selection = window.getSelection();
      if (selection.toString() !== "") {
        const pElement = document.createElement("p");
        pElement.textContent = selection.toString();
        const range = selection.getRangeAt(0);
        range.deleteContents();
        range.insertNode(pElement);
      }
    },
    insertImage() {
      const imageUrl = prompt("Введите URL изображения:");
      if (imageUrl) {
        const imgElement = document.createElement("img");
        imgElement.src = imageUrl;
        this.$refs.editableDiv.appendChild(imgElement);
      }
    },
    copyHTML() {
      const tempElement = document.createElement("textarea");
      tempElement.value = this.$refs.editableDiv.innerHTML;
      document.body.appendChild(tempElement);
      tempElement.select();
      document.execCommand("copy");
      document.body.removeChild(tempElement);
      alert("HTML скопирован в буфер обмена");
    },
  },
}

</script>

<style scoped>

</style>