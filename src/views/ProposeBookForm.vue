<template>
  <MainHeader class="main-header px-5"></MainHeader>

  <main class="px-5">
    <div class="propose-book-container">
      <div class="propose-book-info-container">
        <h2 class="propose-book__title">Book's information</h2>
        <span class="propose-book__description text-m my-1"
          >Fill this form to propose your book
        </span>
      </div>

      <form @submit.prevent="submitForm" class="form-container">
        <FormLabel id="title">
          <template v-slot:labelDescription>Title</template>
          <template v-slot:labelInput>
            <input
              type="text"
              id="title"
              placeholder="Type your book's title"
              maxlength="150"
              required
              class="label__input label__input--caps"
              v-model="title"
            />
          </template>
        </FormLabel>

        <FormLabel id="author">
          <template v-slot:labelDescription>Author</template>
          <template v-slot:labelInput>
            <input
              type="text"
              id="author"
              placeholder="Type author's name"
              maxlength="100"
              required
              class="label__input label__input--caps"
              v-model="author"
            />
          </template>
        </FormLabel>

        <FormLabel id="link">
          <template v-slot:labelDescription>Link</template>
          <template v-slot:labelInput>
            <input
              type="url"
              id="link"
              placeholder="Where can we buy this book?"
              maxlength="300"
              required
              class="label__input"
              v-model="link"
            />
          </template>
        </FormLabel>

        <FormLabel id="synopsis">
          <template v-slot:labelDescription>Synopsis</template>
          <template v-slot:labelInput>
            <textarea
              type="text"
              id="synopsis"
              placeholder="Type a short synopsis about this book... &#10;What does make it interesting? &#10;&#10;&#10;500 characters max."
              maxlength="500"
              required
              class="label__input label__input--extended"
              v-model="synopsis"
            ></textarea>
          </template>
        </FormLabel>

        <ButtonBC
          type="submit"
          :disabled="buttonDisable"
          :class="[
            buttonDisable ? (variant = 'disabled') : (variant = 'primary'),
          ]"
          class="submit-button"
        >
          Let's propose this book!
          <v-icon name="hi-solid-arrow-narrow-right" class="icon-arrow-right" />
        </ButtonBC>

        <div class="propose-book__final--note font-normal">
          <p class="text-xs">
            Remember! you can only propose one book each month
          </p>
        </div>
      </form>
    </div>
  </main>
</template>

<script>
import EventService from "../services/EventService";
import FormLabel from "../components/FormLabel.vue";
import MainHeader from "../components/MainHeader.vue";
import ButtonBC from "../components/ui-components/ButtonComponent.vue";

export default {
  name: "ProposeBookForm",
  components: {
    FormLabel,
    MainHeader,
    ButtonBC,
  },

  data() {
    return {
      title: "",
      author: "",
      link: "",
      synopsis: "",
    };
  },
  computed: {
    titleUpperCase() {
      return this.title.toUpperCase();
    },
    authorUpperCase() {
      return this.author.toUpperCase();
    },
    buttonDisable() {
      return !(this.title && this.author && this.link && this.synopsis);
    },
  },
  methods: {
    async submitForm() {
      try {
        const bookData = {
          book: {
            title: this.titleUpperCase,
            author: this.authorUpperCase,
            synopsis: this.synopsis,
            storeUrl: this.link,
          },
          userId: "abc",
        };
        await EventService.proposeBook(bookData);
        this.$router.push("/");
      } catch (error) {
        alert("Sorry, your book couldn't be posted. Try it later!");
      }
      this.$router.push("/");
    },
  },
};
</script>

<style scoped>
main {
  padding-bottom: 1.6rem;
}
.propose-book-container {
  min-height: 75vh;
  padding: 1.4rem;
  border-radius: 8px;
  background-color: var(--white);
  display: flex;
  flex-direction: column;
  align-items: center;
}
.propose-book-info-container {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}
.form-container {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.propose-book__final--note {
  width: 65%;
  text-align: center;
  margin-top: 1.6rem;
  margin-bottom: 3rem;
  font-family: var(--secondary-font);
}
.submit-button {
  margin-top: 4rem;
  box-shadow: 0 0.2rem 0.4rem rgba(0, 0, 0, 0.25);
}
.icon-arrow-right {
  margin-left: 1rem;
}
</style>
