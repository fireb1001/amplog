<template>
  <div class="page-container">
    <h1>{{ post.title }}</h1>
    <div v-html="$options.filters.amp_it(post.text)"></div>
  </div>
</template>

<script>
export default {
    data () {
        return {}
    },
    head () { // Make it a function to access asyncData
        return {
            title: this.post.title,
            link: [
            { rel: 'canonical', href: '/arab' }
            ]
        }
    },
    async asyncData (context) {
    const db = context.app.db

    var docRef = db.collection('blogposts').doc(context.params.id) // context.params.id
    return new Promise((resolve, reject) => {
        docRef.get().then(function(doc) {
            if (doc.exists) {
                resolve({post: doc.data()})
            } else {
                console.log("No such document!");
                resolve({city: {}})
            }
        }).catch(function(error) {
            console.log("Error getting document:", error);
            reject({})
        });
    })
  },
}
</script>
