To create a GNU Radio Companion command, you can create the file /home/quercus/sdr/bin/grc.sh with the following contents:


```
LIME_INSTALL=$HOME/sdr
LD_LIBRARY_PATH=$LIME_INSTALL/lib/ PYTHONPATH=$LIME_INSTALL/lib/python2.7/dist-packages GRC_BLOCKS_PATH=$LIME_INSTALL/share/gnuradio/grc/blocks $LIME_INSTALL/bin/gnuradio-companion
```

Then run these commands:

```
chmod a+x /home/quercus/sdr/bin/grc.sh
sudo ln -s /home/quercus/sdr/bin/grc.sh /usr/local/bin/grc
```

Now you can run GNU Radio Companion by executing `grc` on the command line.
