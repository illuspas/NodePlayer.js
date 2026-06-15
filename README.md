# NodePlayer.js
NodePlayer.js is a pure JavaScript player based on Asm.js/Wasm. It can play live stream streams using the http-flv/Websocket-flv/whep protocols. It enables low-latency live streaming playback with millisecond-level delay in PC\Android\iOS browsers' Webviews. It supports software decoding of H.264/H.265+AAC/G.711 streams, WebGL video rendering, and WebAudio audio playback. After Chrome 94/iOS 16.4, H264 hardware decoding has been added; after Chrome 107/iOS 17.0, H265 hardware decoding has been added.

## Features
- Support decoding of H.264 video (Baseline, Main, High Profile fully supported, supports decoding of B-frame video)
- Support decoding of H.265 video
- Support filling, aspect ratio, and aspect ratio scaling among three video scaling modes
- Support decoding of AAC audio (LC, HE, HEv2 Profile fully supported)
- Support decoding of G.711 audio of 8kHz PCM_ALAW and PCM_MULAW, traditional monitoring video can be uploaded to the cloud without transcoding
- Support volume adjustment
- Support changing video resolution during playback
- Support changing audio sampling and encoding during playback
- Can set the duration of the playback buffer, with a minimum millisecond limit for low latency
- Also supports http-flv and websocket-flv protocols
- Support HTTPS/WSS encrypted video transmission to ensure the security of video content transmission
- Support HTTP2-flv stream, not subject to the 6-way concurrent limit of Chrome
- Support automatic analysis to determine whether it is supported and use MSE for playback (hardware decoding, supported in most browsers except iOS platforms)
- Support WebWorker multi-core decoding to improve multi-screen playback performance.
- Support WebCodecs hardware decoding API
- Support MediaSourceExtensions hardware decoding
- Support rendering to canvas after hardware decoding with MediaSourceExtensions
- Support SIMD soft decoding instruction set acceleration
- Support rendering of multiple formats such as yuv420, yuv422, yuv444
- Support CN_CDN FLV id 12 h265 standard
- Support EnhanceFlv standard, the h265 standard adopted by ffmpeg 6.1
- Support ManagedMediaSource API for iOS 17.1
- Support standard WHEP protocol playback, using WebRTC for high-performance and low-latency playback (supports NMS, SRS, MediaMTX)
- Support saving the video as mp4 format during playback

## Live Stream Media Server
- [node-media-server](https://github.com/illuspas/Node-Media-Server)

## License
Contact the customer service to obtain the authorization information.
Email: service@nodemedia.cn
