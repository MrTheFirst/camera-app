<template>
    <div class="camera">
        <!-- Camera sensor -->
        <canvas class="camera--sensor" ref="cameraSensor"></canvas>

        <!-- Camera view -->
        <video class="camera--view" ref="camerView" autoplay playsinline></video>

        <!-- Camera output -->
        <img src="//:0" alt="" class="camera--output" ref="cameraOutput">
        <!-- Camera trigger -->
        <button class="camera--trigger" ref="cameraTrigger">Take a picture</button>
    </div>
</template>

<script>
  export default {
    name: 'Camera',
    data() {
      return {
        constraints: {
          video: {
            facingMode: "user"
          },
          audio: false
        },
        track: null,
      }
    },
    mounted() {
      this.cameraStart();
      this.cameraTrigger.addEventListener('click', () => {
        this.takePicture();
      })
    },
    methods: {
      cameraStart() {
        navigator.mediaDevices
          .getUserMedia(this.constraints)
          .then((stream) => {
            console.log(stream);
            this.track = stream.getTracks()[0];
            this.$refs.cameraView.srcObject = stream;
          })
          .catch(function (error) {
            console.error("Oops. Something is broken.", error);
          });
      },
      takePicture() {
        this.$refs.cameraSensor.width = cameraView.videoWidth;
        this.$refs.cameraSensor.height = cameraView.videoHeight;
        this.$refs.cameraSensor.getContext("2d").drawImage(cameraView, 0, 0);
        this.$refs.cameraOutput.src = cameraSensor.toDataURL("image/webp");
        this.$refs.cameraOutput.classList.add("taken");
      },
    }

  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .camera,
    .camera--view,
    .camera--sensor,
    .camera--output {
        position: fixed;
        height: 100%;
        width: 100%;
        object-fit: cover;
    }

    .camera--view,
    .camera--sensor,
    .camera--output {
        transform: scaleX(-1);
        filter: FlipH;
    }

    .camera--trigger {
        width: 200px;
        background-color: black;
        color: white;
        font-size: 16px;
        border-radius: 30px;
        border: none;
        padding: 15px 20px;
        text-align: center;
        box-shadow: 0 5px 10px 0 rgba(0, 0, 0, 0.2);
        position: fixed;
        bottom: 30px;
        left: calc(50% - 100px);
    }

    .taken {
        height: 100px !important;
        width: 100px !important;
        transition: all 0.5s ease-in;
        border: solid 3px white;
        box-shadow: 0 5px 10px 0 rgba(0, 0, 0, 0.2);
        top: 20px;
        right: 20px;
        z-index: 2;
    }
</style>
