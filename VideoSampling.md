#### Camera Feed Sampling

To sample camera feed every X seconds and save the frames at /data folder


##### Image capture
```
#!/bin/bash

DATE=$(date +"%Y-%m-%d")
SAVE_PATH="/data/images/$DATE"
RTSP_URL="rtsp://your_rtsp_url_here"
mkdir -p "$SAVE_PATH"

filename_with_timestamp() {
    echo "$(date +"%Y-%m-%d_%H-%M-%S").jpg"
}

ffmpeg -i "$RTSP_URL" -vf "fps=1,drawtext=text='%{localtime\:%Y-%m-%d %H\\\\\:%M\\\\\:%S}':x=10:y=10:fontsize=24:fontcolor=white" -vframes 1 "$SAVE_PATH/$(filename_with_timestamp)"
```


##### Timelapse video creation
```
#!/bin/bash

YESTERDAY=$(date -d "yesterday" +"%Y-%m-%d")
IMAGES_PATH="/data/images/$YESTERDAY"
VIDEO_PATH="/data/videos/$YESTERDAY.mp4"
mkdir -p "/data/videos"

ffmpeg -framerate 3.33 -pattern_type glob -i "$IMAGES_PATH/*.jpg" -c:v libx264 -pix_fmt yuv420p -r 30 "$VIDEO_PATH"
```

##### Clean up scripts
```
#!/bin/bash

YESTERDAY=$(date -d "yesterday" +"%Y-%m-%d")
IMAGES_PATH="/data/images/$YESTERDAY"
VIDEO_PATH="/data/videos/$YESTERDAY.mp4"

if [[ -f "$VIDEO_PATH" ]]; then
    rm -r "$IMAGES_PATH"
fi
```

##### crontab configuration
```
* * * * * /path/to/script/capture_images.sh
0 0 * * * /path/to/script/create_timelapse.sh
0 1 * * * /path/to/script/cleanup_images.sh
```



