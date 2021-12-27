```
;
; Generic template for mfm_bwm modules
;
module  id=mfm_bwm.<addr>
channel id=in8 desc=in8 events=ingt0,outlt1,ingt1,outgt1,ingt2,out
channel id=in9 desc=in9 events=ingt0,outlt1,ingt1,outgt1,ingt2,out
channel id=in10 desc=in10 events=ingt0,outlt1,ingt1,outgt1,ingt2,out
channel id=in11 desc=in11 events=ingt0,outlt1,ingt1,outgt1,ingt2,out
channel id=im0 desc=im0 events=stop,start
channel id=im1 desc=im1 events=stop,start
channel id=il3 desc=il3 events=dark,bright;
```
