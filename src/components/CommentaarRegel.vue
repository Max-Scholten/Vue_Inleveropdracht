<template>                                                  <!-- CommentaarRegel.vue -->
  <tr>
    <td>
      {{commentaar.auteur}} :
    </td>
    <td>
      <input :value="commentaar.tekst" @keyup='verander'/>
    </td>
  </tr>
</template>

<script>                                                    // CommentaarRegel.vue
export default {
  name: 'CommentaarRegel',
  props: ['index','commentaar'],
  methods:{
    verander($event){                                       // event handler
      console.log('verander', this.commentaar.tekst, ' > ',  $event.target.value)
      this.$emit('veranderId', this.index, this.commentaar.id,
          $event.target.value)                    // vuur event af
    },
  }
}
</script>

<style scoped>
/* styling alleen voor dit component */
input {@apply px-1 ml-1 border-black;}
</style>