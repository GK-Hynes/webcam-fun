# Webcam Fun

A page built to practice manipulating data from the webcam. Built for Wes Bos' [JavaScript 30](https://javascript30.com/) course.

## Notes

### Getting the video from the webcam

Because of security restrictions on getting access to a user's webcam, it must be tied to a secure origin (either localhost or a website that is https). For this reason, this project uses browsersync to run a local server.

The video from the webcam is sent to the video element. By piping it to the canvas element it can be manipulated.

First, select the video, canvas, ctx, the strip where screenshots will go, and the sound effect that will accompany them.

Make a function, `getVideo`. Use `navigator.mediaDevices.getUserMedia` and pass it `video` set to `true` and `audio` set to `false`.

This returns a promise. Call `then` on it and it will give you a `localMediaStream`.

Set the source of the video to be `localMediaStream`.

To receive a live video feed, it must be from a url, bot an object. So use `window.URL.createObjectURL`.

Then call `video.play()`.

If you inspect the video source, it is a `blob`, i.e. the raw data being piped in off the webcam.

Use `catch` to handle the error caused by someone not allowing you to access their webcam.

### Painting the video onto the canvas

Make a function, `paintToCanvas`. Get the width and height of the video. Set the width and height of the canvas to be equal to those of the video.

Using `setInterval`, every 16 milliseconds run `drawImage` on the canvas `ctx` and pass it the video. Start at the top left-hand corner and paint the width and the height.

Return the setInterval so that if you ever need to stop painting you can have access to the interval and call clearInterval on it.

### Taking a photo

Make a function, `takePhoto`.

Set `snap.currentTime` to 0 and run `snap.play` to add the camera shutter sound effect.

Listen for an event on the video element called `canplay` (emitted when the video plays) and run `paintToCanvas`.

To extract the data from the canvas use `canvas.toDataURL` and pass it `"image/jpeg"`. This will give you the data in text-based form.

To get a photo from it, use `document.createElement("a")` and set `link.href` equal to the `data`.

Set the `link`'s attributes to `download` and the filename you want the photo to have.

Set the `link`'s `textContent` to "Download Image".

Finally, take the `strip` and insert the link node right before the `strip.firstChild` (similar to jQuery `prepend`).

Now you can download the image.

To make the image visible on the strip, replace `link.textContent` with `link.innerHTML` and set the `src` to the `data`.

### Manipulate the video with filters
