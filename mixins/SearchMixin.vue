<script>
export default {
  data() {
    return {
      name: this.$route.query.name || '',
      searchQueryReference: this.$route.query.name || '',
      filters: ['All', 'Human', 'Alien', 'Robot', 'Unknown'],
      errors: []
    }
  },
  watch: {
    name() {
      this.errors = []
    }
  },
  methods: {
    onSubmit() {
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
