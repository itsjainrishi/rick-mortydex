<template>
  <div class="form-container">
    <div class="search-form">
      <div class="inner-form">
        <div class="search is-flex">
          <div class="input-field first-wrap">
            <input
              id="search"
              v-model="name"
              type="text"
              placeholder="Search character via name"
              autocomplete="off"
            />
          </div>
          <div class="input-field second-wrap">
            <button
              :class="['btn-search', { 'is-disabled': searchDisabled }]"
              type="button"
              :disabled="searchDisabled"
              @click="onClick"
            >
              <svg
                class="svg-inline--fa fa-search fa-w-16"
                aria-hidden="true"
                data-prefix="fas"
                data-icon="search"
                role="img"
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 512 512"
              >
                <path
                  fill="currentColor"
                  d="M505 442.7L405.3 343c-4.5-4.5-10.6-7-17-7H372c27.6-35.3 44-79.7 44-128C416 93.1 322.9 0 208 0S0 93.1 0 208s93.1 208 208 208c48.3 0 92.7-16.4 128-44v16.3c0 6.4 2.5 12.5 7 17l99.7 99.7c9.4 9.4 24.6 9.4 33.9 0l28.3-28.3c9.4-9.4 9.4-24.6.1-34zM208 336c-70.7 0-128-57.2-128-128 0-70.7 57.2-128 128-128 70.7 0 128 57.2 128 128 0 70.7-57.2 128-128 128z"
                ></path>
              </svg>
            </button>
          </div>
        </div>
        <span v-if="errors.length" class="search-error">{{ errors[0] }}</span>
        <div class="search-filters columns is-multiline">
          <div
            v-if="searchQueryReference.length > 0"
            class="search-query column is-12"
          >
            <div class="search-query-container is-flex">
              <label>
                <span>Search Query:</span>
              </label>
              <label>
                <div class="name box">
                  <span class="query-field">{{ searchQueryReference }}</span>
                  <span class="icon" @click="clearQuery">&times;</span>
                </div>
              </label>
            </div>
          </div>
          <div class="filter column is-12">
            <div class="species-filter is-flex">
              <label>
                <span>Filter via Species:</span>
              </label>
              <label v-for="(filter, index) in filters" :key="index">
                <input
                  v-model="choices"
                  type="radio"
                  :value="filter.toLowerCase()"
                />
                <div class="radio box">
                  <span>{{ filter }}</span>
                </div>
              </label>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    speciesFilter: {
      type: String,
      default: 'all'
    },
    searchQuery: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      choices: this.speciesFilter,
      name: this.searchQuery,
      searchQueryReference: this.searchQuery,
      filters: ['Human', 'Alien', 'Robot', 'Unknown', 'All'],
      errors: []
    }
  },
  computed: {
    searchDisabled() {
      return this.name.length < 4 || this.errors.length > 0
    }
  },
  watch: {
    choices() {
      this.$emit('filterChanged', this.choices)
    },
    name() {
      this.errors = []
    }
  },
  methods: {
    onClick() {
      this.errors = []
      if (this.name.length < 4) {
        this.errors.push('Name should atleast be 4 characters long')
      } else if (!/^[a-z ]+$/i.test(this.name)) {
        this.errors.push('Name can only contain space seperated alphabets')
      }
      if (!this.errors.length > 0) {
        this.$emit('search', this.name)
      }
    },
    clearQuery() {
      this.name = ''
      this.searchQueryReference = ''
      this.$emit('clearQuery', this.name)
    }
  }
}
</script>

<style lang="scss" scoped>
.form-container {
  display: flex;
  justify-content: center;
  align-items: center;
  font-family: 'Poppins', sans-serif;
  background-size: cover;
  background-position: center center;
  padding: 15px;
}

.form-container .search-form {
  width: 100%;
  max-width: 790px;
  margin-bottom: 0;
}

.search.is-flex {
  background: #fff;
  width: 100%;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0px 8px 20px 0px rgba(0, 0, 0, 0.15);
  border-radius: 3px;
  margin-bottom: 20px;

  .input-field {
    height: 68px;

    input {
      height: 100%;
      background: transparent;
      border: 0;
      display: block;
      width: 100%;
      padding: 10px 32px;
      font-size: 16px;
      color: #555;

      &:hover,
      &:focus {
        box-shadow: none;
        outline: 0;
        border-color: #fff;
      }
    }
  }
}

.first-wrap {
  flex-grow: 1;
}

.search-error {
  color: red;
  margin: 10px 0;
}

.second-wrap {
  width: 74px;

  .btn-search {
    height: 100%;
    width: 100%;
    white-space: nowrap;
    color: #fff;
    border: 0;
    cursor: pointer;
    background: darkseagreen;
    transition: all 0.2s ease-out, color 0.2s ease-out;

    svg {
      width: 16px;
    }

    &:hover {
      background: #50c058;
    }

    &:focus {
      outline: 0;
      box-shadow: none;
    }
  }
}

.btn-search.is-disabled {
  cursor: not-allowed;
}

.search-query-container {
  align-items: center;
  font-weight: 700;
  color: darkseagreen;

  .name {
    margin-left: 10px;
  }
}

.is-flex {
  align-items: center;
}

.filter {
  color: darkseagreen;
  font-weight: 700;

  input[type='radio'] {
    display: none;
    &:checked {
      + .box {
        background-color: darkseagreen;
        span {
          color: white;
          &:before {
            opacity: 1;
          }
        }
      }
    }
  }
}

.name.box {
  width: 100px;

  .query-field {
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
    max-width: 70px;
  }
}

.box {
  width: 100px;
  height: 50px;
  background-color: #fff;
  display: inline-flex;
  align-items: center;
  text-align: center;
  cursor: pointer;
  position: relative;
  font-weight: 700;
  box-shadow: 0px 8px 20px 0px rgba(0, 0, 0, 0.15);
  margin-left: 10px;
  padding: 5px;

  span:not(.icon) {
    position: absolute;
    left: 10px;
    right: 10px;
    transition: all 300ms ease;
    font-size: 1em;
    user-select: none;
    color: green;
  }

  span.icon {
    position: absolute;
    right: 0;
    font-size: 1.5rem;
    color: red;
  }
}

.radio:hover {
  color: #50c058;
}

@media screen and (max-width: 992px) {
  .form-container .search-form .inner-form .input-field {
    height: 50px;
  }
}

@media screen and (max-width: 767px) {
  .inner-form {
    flex-wrap: wrap;
    padding: 20px;

    .input-field {
      margin-bottom: 20px;
      border-bottom: 1px solid rgba(0, 0, 0, 0.1);

      input {
        padding: 10px 15px;
      }
    }
  }

  .first-wrap {
    width: 100%;
    border-right: 0;
  }

  .first-wrap {
    width: 100%;
    margin-bottom: 30px;
    input {
      border: 1px solid rgba(255, 255, 255, 0.3);
    }
  }

  .second-wrap {
    margin-bottom: 0;
    width: 100%;
  }
}
</style>
