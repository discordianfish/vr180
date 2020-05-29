# vr180

## RPI Recorder
- 2x [ELP Webcam USB3.0 Compatible with Sensor Sony IMX291](https://smile.amazon.de/gp/product/B07KMW5TRS/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1)
- 2x [180° M12 Lens](https://smile.amazon.de/gp/product/B07VXGGK35/ref=ppx_yo_dt_b_asin_title_o06_s00?ie=UTF8&psc=1)
- RPI 4

### Streaming
```
ffmpeg -f v4l2 -input_format mjpeg -i /dev/video0 -f mpegts -codec copy udp://192.168.1.111:1234
ffmpeg -f v4l2 -input_format mjpeg -i /dev/video2 -f rtp -an -codec copy udp://192.168.1.111:1234                           
ffmpeg -f v4l2 -i /dev/video0 -i /dev/video2 -filter_complex '[0:v]pad=iw*2:ih[int];[int][1:v]overlay=W/2:0[vid]' udp://192.168.1.111:1234
```

### Recording
```
ffmpeg -f v4l2 -i /dev/video0 -i /dev/video2 -filter_complex "[1:v][0:v]scale2ref=oh*mdar:ih[1v][0v];[2:v][0v]scale2ref=oh*mdar:ih[2v][0v];[0v][1v][2v]hstack=3,scale='2*trunc(iw/2)':'2*trunc(ih/2)'" -codec copy test.mkv                       
```
