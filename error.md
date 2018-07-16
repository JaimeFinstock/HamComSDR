[INFO] Reference clock 30.72 MHz
[INFO] Device name: LimeSDR-USB
[INFO] Reference: 30.72 MHz
[INFO] LMS7002M calibration values caching Disable
[INFO] RX LPF configured
gr::log :INFO: audio source - Audio sink arch: oss
audio_oss_sink: /dev/dsp: No such file or directory
Traceback (most recent call last):
  File "/home/quercus/Desktop/top_block.py", line 221, in <module>
    main()
  File "/home/quercus/Desktop/top_block.py", line 209, in main
    tb = top_block_cls()
  File "/home/quercus/Desktop/top_block.py", line 141, in __init__
    self.audio_sink_0 = audio.sink(48000, '', True)
  File "/home/quercus/sdr/lib/python2.7/dist-packages/gnuradio/audio/audio_swig.py", line 152, in make
    return _audio_swig.sink_make(*args, **kwargs)
RuntimeError: audio_oss_sink
libusb: warning [libusb_exit] application left some devices open

>>> Done
