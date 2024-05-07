<script>
        // Function to capture image from webcam and send to Flask backend
        function captureImage() {
            var canvas = document.createElement('canvas');
            var context = canvas.getContext('2d');
            var video = document.createElement('video');
            var imagePreview = document.getElementById('imagePreview');
            navigator.mediaDevices.getUserMedia({ video: true })
                .then(function (stream) {
                    video.srcObject = stream;
                    video.onloadedmetadata = function (e) {
                        video.play();
                        canvas.width = video.videoWidth;
                        canvas.height = video.videoHeight;
                        context.drawImage(video, 0, 0, canvas.width, canvas.height);
                        var dataURL = canvas.toDataURL('image/png');
                        imagePreview.src = dataURL;
                        // Send image data to Flask backend
                        fetch('/gesture', {
                            method: 'POST',
                            headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
                            body: 'image=' + dataURL
                        })
                            .then(response => response.text())
                            .then(result => {
                                // Display the result of gesture recognition
                                alert(result);
                            })
                            .catch(error => console.error('Error:', error));
                    };
                })
                .catch(function (err) { console.error(err.name + ": " + err.message); });
        }

        // Launch software when button is clicked
        function launchSoftware() {
            captureImage();
        }
    </script>