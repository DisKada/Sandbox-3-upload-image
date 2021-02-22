<template>
  <div>
    <div>
      <p>Upload an image to Firebase:</p>
      <input type="file" @change="previewImage" accept="image/*" class="form-control">
    </div>
    <div>
      <p>Progress: {{uploadValue.toFixed()+"%"}}
      <progress id="progress" :value="uploadValue" max="100" ></progress>  </p>
    </div>
    <div v-if="imageData!=null">
        <img class="preview" :src="picture">
        <br>
      <button @click="onUpload" class="btn btn-success">Upload</button>
    </div>
  </div>
</template>

<script>
import firebase from 'firebase/app'
import 'firebase/storage'

export default {
  name: 'Upload',
  data () {
    return {
      imageData: null,
      picture: null,
      uploadValue: 0,
      fileType: null,
      fileSize: 0
    }
  },
  methods: {
    previewImage (event) {
      this.uploadValue = 0
      this.picture = null
      this.imageData = event.target.files[0]
      this.fileType = event.target.files[0].type
      this.fileSize = event.target.files[0].size
      console.log(event.target.files[0])
    },
    onUpload () {
      this.picture = null
      if ((this.fileType === 'image/png' || this.fileType === 'image/jpg' || this.fileType === 'image/jpeg') && this.fileSize <= 100_000) {
        const storageRef = firebase.storage().ref(`${new Date().getTime()}-${this.imageData.name}`).put(this.imageData)
        storageRef.on('state_changed', snapshot => {
          this.uploadValue = (snapshot.bytesTransferred / snapshot.totalBytes) * 100
        }, error => { console.log(error.message) },
        () => {
          this.uploadValue = 100
          storageRef.snapshot.ref.getDownloadURL().then((url) => {
            this.picture = url
            console.log(url)
          })
        })
      } else {
        console.log('file type must be png/jpg/jpeg and image size too more than 100Kb')
      }
    }
  }
}
</script>
