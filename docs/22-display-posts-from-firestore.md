# Display posts from Firestore

1. Remove mock data from `src/pages/FeedPage.vue`

   ```diff
     <script>
   - import firebase from '@lib/firebase';
     import NewPostForm from '@components/NewPostForm.vue';

     export default {
       components: {
         NewPostForm,
       },

       data() {
         return {
   +       posts: [],
   -       posts: [
   -         {
   -           id: 'BHIvuCPC0xir16icwl7T',
   -           caption: 'hello world',
   -           photoURL:
   -             'http://localhost:9199/v0/b/instagram-clone-2d8bc.appspot.com/o/photos%2Fjn1M0FbcmcB54A6O8t6bEX4aTHlj%2F5039dadab9534994bc7748473cc58d31.jpeg?alt=media&token=979daac3-ed9a-4880-befb-cab26c96e110',
   -           datePosted: new firebase.firestore.Timestamp(1622246105, 412),
   -           likesCount: 10,
   -           commentsCount: 1,
   -           latestComments: [
   -             {
   -               id: 'abc123',
   -               body: 'this is cool!',
   -               author: {
   -                 id: 'jn1M0FbcmcB54A6O8t6bEX4aTHlj',
   -                 displayName: 'Arnelle Balane',
   -               },
   -             },
   -           ],
   -           author: {
   -             id: 'jn1M0FbcmcB54A6O8t6bEX4aTHlj',
   -             displayName: 'Arnelle Balane',
   -             photoURL:
   -               'https://res.cloudinary.com/arnellebalane/image/fetch/w_256,f_webp,q_85/https://arnellebalane.com/arnelle-avatar.jpg',
   -           },
   -         },
   -         {
   -           id: 'BHIvuCPC0xir16icwl7a',
   -           caption: 'hello world',
   -           photoURL:
   -             'http://localhost:9199/v0/b/instagram-clone-2d8bc.appspot.com/o/photos%2Fjn1M0FbcmcB54A6O8t6bEX4aTHlj%2F5039dadab9534994bc7748473cc58d31.jpeg?alt=media&token=979daac3-ed9a-4880-befb-cab26c96e110',
   -           datePosted: new firebase.firestore.Timestamp(1622246105, 412),
   -           likesCount: 20,
   -           commentsCount: 10,
   -           latestComments: [
   -             {
   -               id: 'abc1235',
   -               body: 'this is cool!',
   -               author: {
   -                 id: 'jn1M0FbcmcB54A6O8t6bEX4aTHlj',
   -                 displayName: 'Arnelle Balane',
   -               },
   -             },
   -             {
   -               id: 'abc1234',
   -               body: 'this is awesome!',
   -               author: {
   -                 id: 'jn1M0FbcmcB54A6O8t6bEX4aTHlj',
   -                 displayName: 'Arnelle Balane',
   -               },
   -             },
   -           ],
   -           author: {
   -             id: 'jn1M0FbcmcB54A6O8t6bEX4aTHlj',
   -             displayName: 'Arnelle Balane',
   -             photoURL:
   -               'https://res.cloudinary.com/arnellebalane/image/fetch/w_256,f_webp,q_85/https://arnellebalane.com/arnelle-avatar.jpg',
   -           },
   -         },
   -       ],
         };
       },
     };
     </script>
   ```

1. Query posts data inside `mounted` lifecycle hook in `src/pages/FeedPage.vue`

   ```diff
     <script>
   + import { db } from '@lib/firebase';
     import NewPostForm from '@components/NewPostForm.vue';
     import Feed from '@components/Feed.vue';

     export default {
       components: {
         NewPostForm,
         Feed,
       },

       data() {
         return {
           posts: [],
         };
       },

   +   mounted() {
   +     db.collection('posts')
   +       .orderBy('datePosted', 'desc')
   +       .onSnapshot((snapshot) => {
   +         this.posts = snapshot.docs
   +           .filter((doc) => !doc.metadata.hasPendingWrites)
   +           .map((doc) => ({ ...doc.data(), id: doc.id }));
   +       });
   +   },
     };
     </script>
   ```

### Switch to completed branch for this step:

```bash
git checkout 22-display-posts-from-firestore
```

---

- [**Return to previous step**](21-implement-feed-and-post-components.md)
- [**Proceed to next step**]()
