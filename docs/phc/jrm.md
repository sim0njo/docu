```
;
; Generic template for jrm modules
;
module  id=jrm.<addr>
channel id=out0 desc=out0
channel id=out1 desc=out1
channel id=out2 desc=out2
channel id=out3 desc=out3
channel id=out4 desc=out4
channel id=out5 desc=out5
channel id=out6 desc=out6
channel id=out7 desc=out7
channel id=fb0  desc=fb0 events=upon,downon,upoff,downoff
channel id=fb1  desc=fb1 events=upon,downon,upoff,downoff
channel id=fb2  desc=fb2 events=upon,downon,upoff,downoff
channel id=fb3  desc=fb3 events=upon,downon,upoff,downoff
channel id=fb4  desc=fb4 events=timeron,timerabort,timeroff
channel id=fb5  desc=fb5 events=timeron,timerabort,timeroff
channel id=fb6  desc=fb6 events=timeron,timerabort,timeroff
channel id=fb7  desc=fb7 events=timeron,timerabort,timeroff;
```
