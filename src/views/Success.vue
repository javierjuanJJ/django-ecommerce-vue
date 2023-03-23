<template>
  <div className="page-success">
    <div className="columns is-multiline">
      <div className="column is-12">
        <h1 className="title">Thank you</h1>

        <p>Your order will be processed within 48 hours</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Success',
  mounted() {
    document.title = 'Success | Djackets'
  },
}
</script>