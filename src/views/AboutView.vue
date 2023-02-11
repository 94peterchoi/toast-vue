<template>
  <div id="editor"></div>
</template>

<script>
  import Editor from '@toast-ui/editor';
  import '@toast-ui/editor/dist/toastui-editor.css'; // Editor's Style

  export default {
    data() {
      return {
        editor: null
      }
    },
    mounted() {
      this.editor = new Editor({
        el: document.querySelector('#editor'),
        height: '500px',
        initialEditType: 'wysiwyg',
        hooks: {
          addImageBlobHook: async (blob, callback) => {
            // 1. 다른 서버에 이미지를 업로드
            const uploadResult = await this.uploadImage(blob);
            // 2. 1에서 업로드 된 이미지를 접근할 수 있는 url 세팅
            callback(uploadResult.imageAccessUrl);
          }
        }
      });
    },
    methods: {
      async uploadImage (blob) {
        const formData = new FormData();
        formData.append('image', blob);

        const options = {
            method: 'POST',
            body: formData,
        }

        let response = await fetch('http://localhost:8080/upload', options);
        let result = response.json();

        return result;
      }
    }
  }


</script>