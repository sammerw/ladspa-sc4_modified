# ladspa-sc4_modified


This is a modified patch for the swh-plugins of LADSPA, specifically the sc4_1882 compressor.

The aim of modding the sc4 compressor is to bring forth the stereo effect more clearly than, say, a stereo multiplier or widener would.

Download the source of swh-plugins and apply the patch_sc4.txt to the sc4_1882.xml file.

Here is command to patch:

patch sc4_1882.xml < patch_sc4.txt


Then rebuild the sc4, by typing make.


The generated sc4_1882.so will be in .libs folder.


Copy the modified sc4_1882.so file to /usr/lib/ladspa or to /usr/local/lib/ladspa


Then in default.pa Pulseaudio config file, chained 2 modified sc4_1882.so compressors one with Peak, 2nd with RMS.



load-module module-alsa-sink device=default sink_name=output




load-module module-ladspa-sink sink_name=ladspa_output.sc42 label=sc4 plugin=sc4_1882 master=output control=0,2,650,-30,1,6,0 &#12288;




load-module module-ladspa-sink sink_name=ladspa_output.sc4 label=sc4 plugin=sc4_1882 master=ladspa_output.sc42 control=1,180,350,-27,1,6,0 &#12288;




...

set-default-sink ladspa_output.sc4


Restart pulseaudio with

pulseaudio --kill

pulseaudio --start


If pulseaudio is in system mode you will need sudo privileges to restart pulseaudio.

Finally, listen to any 'stereo' song, and you might get a hear a 'depth' to the music.




Additionally, the above chain of compressors can be "double chained" like this to multiply the effect of stereo :



load-module module-alsa-sink device=default sink_name=output


load-module module-ladspa-sink sink_name=ladspa_output.sc44 label=sc4 plugin=sc4_1882 master=output control=0,2,650,-30,1,7,0


load-module module-ladspa-sink sink_name=ladspa_output.sc43 label=sc4 plugin=sc4_1882 master=ladspa_output.sc44 control=1,180,350,-26.67,1,7,0


load-module module-ladspa-sink sink_name=ladspa_output.sc42 label=sc4 plugin=sc4_1882 master=ladspa_output.sc43 control=0,2,650,-30,2,7,0


load-module module-ladspa-sink sink_name=ladspa_output.sc4 label=sc4 plugin=sc4_1882 master=ladspa_output.sc42 control=1,180,350,-26.67,2,7,0


....



set-default-sink ladspa_output.sc4
