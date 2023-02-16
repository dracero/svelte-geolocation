<script>
    import { onMount } from 'svelte';
  
    onMount(() => {
      if (typeof window !== 'undefined') {
        navigator.geolocation.getCurrentPosition(function (position) {
        var latitude = position.coords.latitude;
        var longitude = position.coords.longitude;
        navigator.mediaDevices
          .getUserMedia({ video: true })
          .then(function (stream) {
            var video = document.getElementById("video");
            video.srcObject = stream;
            video.play();
          })
          .catch(function (err) {
            console.log("An error occurred: " + err);
          });

        var canvas = document.getElementById("canvas");
        var ctx = canvas.getContext("2d");

        function draw() {
          ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
          ctx.font = "24px Arial";
          ctx.fillStyle = "red";
          ctx.fillText(
            `Latitude: ${latitude}, Longitude: ${longitude}`,
            10,
            50
          );
          requestAnimationFrame(draw);
        }

        video.addEventListener("canplay", function (e) {
          canvas.width = video.videoWidth;
          canvas.height = video.videoHeight;
          draw();
        });
      });
      }
    });
  </script>
  
  <style>
    #video {
      position: absolute;
      top: 0;
      left: 0;
    }

    #canvas {
      position: absolute;
      top: 0;
      left: 0;
    }
  </style>
  <!-- svelte-ignore a11y-media-has-caption -->
  <video id="video" autoplay></video>
  <canvas id="canvas"></canvas>
  