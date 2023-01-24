<template>
  <div class="app">
    <!-- Content -->
    <section class="section">
      <div class="container">
        <!-- Error -->
        <Message v-show="message" :message="message" />

        <!-- Create Note -->
        <CreateNote @create-note="createNote" />

        <!-- Header -->
        <div class="header">
          <p class="title">{{ title }}</p>
          <Search
            :searchLetter="searchLetter"
            @search-note="searchLetter = $event"
          />
          <div class="direction">
            <button @click="grid = true" :class="{ active: grid }">
              <svg
                width="24px"
                height="24px"
                viewBox="0 0 24 24"
                version="1.1"
                xmlns="http://www.w3.org/2000/svg"
                xmlns:xlink="http://www.w3.org/1999/xlink"
              >
                <defs>
                  <path
                    d="M3,3 L10,3 C10.55,3 11,3.45 11,4 L11,10 C11,10.55 10.55,11 10,11 L3,11 C2.45,11 2,10.55 2,10 L2,4 C2,3.45 2.45,3 3,3 Z M3,13 L10,13 C10.55,13 11,13.45 11,14 L11,20 C11,20.55 10.55,21 10,21 L3,21 C2.45,21 2,20.55 2,20 L2,14 C2,13.45 2.45,13 3,13 Z M14,3 L21,3 C21.55,3 22,3.45 22,4 L22,10 C22,10.55 21.55,11 21,11 L14,11 C13.45,11 13,10.55 13,10 L13,4 C13,3.45 13.45,3 14,3 Z M14,13 L21,13 C21.55,13 22,13.45 22,14 L22,20 C22,20.55 21.55,21 21,21 L14,21 C13.45,21 13,20.55 13,20 L13,14 C13,13.45 13.45,13 14,13 Z M9,9 L9,5 L4,5 L4,9 L9,9 Z M9,19 L9,15 L4,15 L4,19 L9,19 Z M20,9 L20,5 L15,5 L15,9 L20,9 Z M20,19 L20,15 L15,15 L15,19 L20,19 Z"
                    id="path-1"
                  />
                </defs>
                <g
                  id="grid_view_24px"
                  stroke="none"
                  stroke-width="1"
                  fill="none"
                  fill-rule="evenodd"
                >
                  <polygon
                    id="bounds"
                    fill-opacity="0"
                    fill="#FFFFFF"
                    points="0 0 24 0 24 24 0 24"
                  />
                  <mask id="mask-2" fill="white">
                    <use xlink:href="#path-1" />
                  </mask>
                  <use
                    id="icon"
                    fill="#5f6368"
                    fill-rule="nonzero"
                    xlink:href="#path-1"
                  />
                </g>
              </svg>
            </button>
            <button @click="grid = false" :class="{ active: !grid }">
              <svg
                width="24px"
                height="24px"
                viewBox="0 0 24 24"
                version="1.1"
                xmlns="http://www.w3.org/2000/svg"
              >
                <g
                  id="list_view_24px"
                  stroke="none"
                  stroke-width="1"
                  fill="none"
                  fill-rule="evenodd"
                >
                  <polygon
                    id="bounds"
                    fill-opacity="0"
                    fill="#FFFFFF"
                    points="0 0 24 0 24 24 0 24"
                  />
                  <path
                    d="M20,9 L4,9 L4,5 L20,5 L20,9 Z M20,19 L4,19 L4,15 L20,15 L20,19 Z M3,3 C2.45,3 2,3.45 2,4 L2,10 C2,10.55 2.45,11 3,11 L21,11 C21.55,11 22,10.55 22,10 L22,4 C22,3.45 21.55,3 21,3 L3,3 Z M3,13 C2.45,13 2,13.45 2,14 L2,20 C2,20.55 2.45,21 3,21 L21,21 C21.55,21 22,20.55 22,20 L22,14 C22,13.45 21.55,13 21,13 L3,13 Z"
                    id="icon"
                    fill="#5f6368"
                    fill-rule="nonzero"
                  />
                </g>
              </svg>
            </button>
          </div>
        </div>

        <!-- Notes -->
        <Notes :grid="grid" :notes="filteredNotes" @remove-note="removeNote" />
      </div>
    </section>

    <!-- Footer -->
  </div>
</template>

<script>
import Message from "@/components/Message.vue";
import CreateNote from "@/components/CreateNote.vue";
import Notes from "@/components/Notes.vue";
import Search from "@/components/Search.vue";
import currentDate from "@/utils/helpers/currentDate";

export default {
  name: "App",
  components: {
    Message,
    CreateNote,
    Notes,
    Search,
  },
  computed: {
    filteredNotes() {
      if (!this.searchLetter) return this.notes;
      else {
        return this.notes.filter((note) =>
          note.title.toLowerCase().includes(this.searchLetter)
        );
      }
    },
  },
  data() {
    return {
      title: "Vue Notes",
      message: null,
      grid: true,
      searchLetter: "",
      notes: [
        {
          id: 1,
          title: "First Note",
          description: "First Note Description",
          date: currentDate(),
        },
        {
          id: 11,
          title: "Second Note",
          description: "Second Note Description",
          date: currentDate(),
        },
        {
          id: 111,
          title: "Third Note",
          description: "Third Note Description",
          date: currentDate(),
        },
      ],
    };
  },
  methods: {
    createNote(note) {
      const { title, description } = note;

      if (title && description) {
        this.notes.push({
          title,
          description,
          date: currentDate(),
        });

        this.message = null;
      } else {
        this.message = "Empty Note";
      }
    },
    removeNote(id) {
      this.notes = this.notes.filter((note) => note.id != id);
    },
  },
};
</script>

<style lang="scss">
@import url(https://fonts.googleapis.com/css2?family=DM+Sans&family=Montserrat&family=Merriweather+Sans&family=Poppins:wght@600;700&display=swap);
body {
  overflow-y: scroll;
}

.app {
  font-family: "Montserrat";
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

button,
svg {
  cursor: pointer;
}
</style>

<style lang="scss" scoped>
.section {
  .container {
    width: 65%;
    margin: 25px auto;

    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 30px 0;

      .title {
        font-size: 20px;
        font-weight: 600;
      }

      .direction {
        display: flex;
        justify-content: flex-end;
        align-items: center;

        button {
          background: transparent;
          border: none;
          font-size: 14px;
          margin-left: 15px;
          font-weight: 600;
          font-family: "Montserrat";

          svg {
            path {
              fill: rgb(225, 225, 225);
            }
          }

          &.active {
            svg {
              path {
                fill: rgb(50, 50, 225);
              }
            }
          }
        }
      }
    }
  }
}
</style>
