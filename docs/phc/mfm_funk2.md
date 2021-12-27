```
;
; Generic template for mfm_funk2 modules, these take 8 consecutive addresses !!!
;
module  id=mfm_funk2.<addr>
channel id=in0   desc=in0  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in1   desc=in1  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in2   desc=in2  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in3   desc=in3  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in4   desc=in4  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in5   desc=in5  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in6   desc=in6  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in7   desc=in7  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in8   desc=in8  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in9   desc=in9  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in10  desc=in10 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in11  desc=in11 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in12  desc=in12 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in13  desc=in13 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in14  desc=in14 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=out0  desc=out0
channel id=out1  desc=out1
channel id=out2  desc=out2
channel id=out3  desc=out3
channel id=out4  desc=out4
channel id=out5  desc=out5
channel id=out6  desc=out6
channel id=out7  desc=out7
channel id=out8  desc=out8
channel id=out9  desc=out9
channel id=out10 desc=out10
channel id=out11 desc=out11
channel id=out12 desc=out12
channel id=out13 desc=out13
channel id=out14 desc=out14
channel id=fb0   desc=fb0  events=on,off
channel id=fb1   desc=fb1  events=on,off
channel id=fb2   desc=fb2  events=on,off
channel id=fb3   desc=fb3  events=on,off
channel id=fb4   desc=fb4  events=on,off
channel id=fb5   desc=fb5  events=on,off
channel id=fb6   desc=fb6  events=on,off
channel id=fb7   desc=fb7  events=on,off
channel id=fb8   desc=fb8  events=on,off
channel id=fb9   desc=fb9  events=on,off
channel id=fb10  desc=fb10 events=on,off
channel id=fb11  desc=fb11 events=on,off
channel id=fb12  desc=fb12 events=on,off
channel id=fb13  desc=fb13 events=on,off
channel id=fb14  desc=fb14 events=on,off;

module  id=mfm_funk2.<addr + 1>
channel id=in0   desc=in0  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in1   desc=in1  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in2   desc=in2  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in3   desc=in3  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in4   desc=in4  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in5   desc=in5  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in6   desc=in6  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in7   desc=in7  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in8   desc=in8  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in9   desc=in9  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in10  desc=in10 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in11  desc=in11 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in12  desc=in12 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in13  desc=in13 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in14  desc=in14 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=out0  desc=out0
channel id=out1  desc=out1
channel id=out2  desc=out2
channel id=out3  desc=out3
channel id=out4  desc=out4
channel id=out5  desc=out5
channel id=out6  desc=out6
channel id=out7  desc=out7
channel id=out8  desc=out8
channel id=out9  desc=out9
channel id=out10 desc=out10
channel id=out11 desc=out11
channel id=out12 desc=out12
channel id=out13 desc=out13
channel id=out14 desc=out14
channel id=fb0   desc=fb0  events=on,off
channel id=fb1   desc=fb1  events=on,off
channel id=fb2   desc=fb2  events=on,off
channel id=fb3   desc=fb3  events=on,off
channel id=fb4   desc=fb4  events=on,off
channel id=fb5   desc=fb5  events=on,off
channel id=fb6   desc=fb6  events=on,off
channel id=fb7   desc=fb7  events=on,off
channel id=fb8   desc=fb8  events=on,off
channel id=fb9   desc=fb9  events=on,off
channel id=fb10  desc=fb10 events=on,off
channel id=fb11  desc=fb11 events=on,off
channel id=fb12  desc=fb12 events=on,off
channel id=fb13  desc=fb13 events=on,off
channel id=fb14  desc=fb14 events=on,off;

module  id=mfm_funk2.<addr + 2>
channel id=in0   desc=in0  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in1   desc=in1  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in2   desc=in2  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in3   desc=in3  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in4   desc=in4  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in5   desc=in5  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in6   desc=in6  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in7   desc=in7  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in8   desc=in8  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in9   desc=in9  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in10  desc=in10 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in11  desc=in11 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in12  desc=in12 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in13  desc=in13 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in14  desc=in14 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=out0  desc=out0
channel id=out1  desc=out1
channel id=out2  desc=out2
channel id=out3  desc=out3
channel id=out4  desc=out4
channel id=out5  desc=out5
channel id=out6  desc=out6
channel id=out7  desc=out7
channel id=out8  desc=out8
channel id=out9  desc=out9
channel id=out10 desc=out10
channel id=out11 desc=out11
channel id=out12 desc=out12
channel id=out13 desc=out13
channel id=out14 desc=out14
channel id=fb0   desc=fb0  events=on,off
channel id=fb1   desc=fb1  events=on,off
channel id=fb2   desc=fb2  events=on,off
channel id=fb3   desc=fb3  events=on,off
channel id=fb4   desc=fb4  events=on,off
channel id=fb5   desc=fb5  events=on,off
channel id=fb6   desc=fb6  events=on,off
channel id=fb7   desc=fb7  events=on,off
channel id=fb8   desc=fb8  events=on,off
channel id=fb9   desc=fb9  events=on,off
channel id=fb10  desc=fb10 events=on,off
channel id=fb11  desc=fb11 events=on,off
channel id=fb12  desc=fb12 events=on,off
channel id=fb13  desc=fb13 events=on,off
channel id=fb14  desc=fb14 events=on,off;

module  id=mfm_funk2.<addr + 3>
channel id=in0   desc=in0  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in1   desc=in1  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in2   desc=in2  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in3   desc=in3  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in4   desc=in4  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in5   desc=in5  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in6   desc=in6  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in7   desc=in7  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in8   desc=in8  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in9   desc=in9  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in10  desc=in10 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in11  desc=in11 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in12  desc=in12 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in13  desc=in13 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in14  desc=in14 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=out0  desc=out0
channel id=out1  desc=out1
channel id=out2  desc=out2
channel id=out3  desc=out3
channel id=out4  desc=out4
channel id=out5  desc=out5
channel id=out6  desc=out6
channel id=out7  desc=out7
channel id=out8  desc=out8
channel id=out9  desc=out9
channel id=out10 desc=out10
channel id=out11 desc=out11
channel id=out12 desc=out12
channel id=out13 desc=out13
channel id=out14 desc=out14
channel id=fb0   desc=fb0  events=on,off
channel id=fb1   desc=fb1  events=on,off
channel id=fb2   desc=fb2  events=on,off
channel id=fb3   desc=fb3  events=on,off
channel id=fb4   desc=fb4  events=on,off
channel id=fb5   desc=fb5  events=on,off
channel id=fb6   desc=fb6  events=on,off
channel id=fb7   desc=fb7  events=on,off
channel id=fb8   desc=fb8  events=on,off
channel id=fb9   desc=fb9  events=on,off
channel id=fb10  desc=fb10 events=on,off
channel id=fb11  desc=fb11 events=on,off
channel id=fb12  desc=fb12 events=on,off
channel id=fb13  desc=fb13 events=on,off
channel id=fb14  desc=fb14 events=on,off;

module  id=mfm_funk2.<addr + 4>
channel id=in0   desc=in0  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in1   desc=in1  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in2   desc=in2  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in3   desc=in3  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in4   desc=in4  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in5   desc=in5  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in6   desc=in6  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in7   desc=in7  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in8   desc=in8  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in9   desc=in9  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in10  desc=in10 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in11  desc=in11 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in12  desc=in12 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in13  desc=in13 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in14  desc=in14 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=out0  desc=out0
channel id=out1  desc=out1
channel id=out2  desc=out2
channel id=out3  desc=out3
channel id=out4  desc=out4
channel id=out5  desc=out5
channel id=out6  desc=out6
channel id=out7  desc=out7
channel id=out8  desc=out8
channel id=out9  desc=out9
channel id=out10 desc=out10
channel id=out11 desc=out11
channel id=out12 desc=out12
channel id=out13 desc=out13
channel id=out14 desc=out14
channel id=fb0   desc=fb0  events=on,off
channel id=fb1   desc=fb1  events=on,off
channel id=fb2   desc=fb2  events=on,off
channel id=fb3   desc=fb3  events=on,off
channel id=fb4   desc=fb4  events=on,off
channel id=fb5   desc=fb5  events=on,off
channel id=fb6   desc=fb6  events=on,off
channel id=fb7   desc=fb7  events=on,off
channel id=fb8   desc=fb8  events=on,off
channel id=fb9   desc=fb9  events=on,off
channel id=fb10  desc=fb10 events=on,off
channel id=fb11  desc=fb11 events=on,off
channel id=fb12  desc=fb12 events=on,off
channel id=fb13  desc=fb13 events=on,off
channel id=fb14  desc=fb14 events=on,off;

module  id=mfm_funk2.<addr + 5>
channel id=in0   desc=in0  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in1   desc=in1  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in2   desc=in2  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in3   desc=in3  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in4   desc=in4  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in5   desc=in5  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in6   desc=in6  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in7   desc=in7  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in8   desc=in8  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in9   desc=in9  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in10  desc=in10 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in11  desc=in11 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in12  desc=in12 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in13  desc=in13 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in14  desc=in14 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=out0  desc=out0
channel id=out1  desc=out1
channel id=out2  desc=out2
channel id=out3  desc=out3
channel id=out4  desc=out4
channel id=out5  desc=out5
channel id=out6  desc=out6
channel id=out7  desc=out7
channel id=out8  desc=out8
channel id=out9  desc=out9
channel id=out10 desc=out10
channel id=out11 desc=out11
channel id=out12 desc=out12
channel id=out13 desc=out13
channel id=out14 desc=out14
channel id=fb0   desc=fb0  events=on,off
channel id=fb1   desc=fb1  events=on,off
channel id=fb2   desc=fb2  events=on,off
channel id=fb3   desc=fb3  events=on,off
channel id=fb4   desc=fb4  events=on,off
channel id=fb5   desc=fb5  events=on,off
channel id=fb6   desc=fb6  events=on,off
channel id=fb7   desc=fb7  events=on,off
channel id=fb8   desc=fb8  events=on,off
channel id=fb9   desc=fb9  events=on,off
channel id=fb10  desc=fb10 events=on,off
channel id=fb11  desc=fb11 events=on,off
channel id=fb12  desc=fb12 events=on,off
channel id=fb13  desc=fb13 events=on,off
channel id=fb14  desc=fb14 events=on,off;

module  id=mfm_funk2.<addr + 6>
channel id=in0   desc=in0  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in1   desc=in1  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in2   desc=in2  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in3   desc=in3  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in4   desc=in4  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in5   desc=in5  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in6   desc=in6  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in7   desc=in7  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in8   desc=in8  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in9   desc=in9  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in10  desc=in10 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in11  desc=in11 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in12  desc=in12 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in13  desc=in13 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in14  desc=in14 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=out0  desc=out0
channel id=out1  desc=out1
channel id=out2  desc=out2
channel id=out3  desc=out3
channel id=out4  desc=out4
channel id=out5  desc=out5
channel id=out6  desc=out6
channel id=out7  desc=out7
channel id=out8  desc=out8
channel id=out9  desc=out9
channel id=out10 desc=out10
channel id=out11 desc=out11
channel id=out12 desc=out12
channel id=out13 desc=out13
channel id=out14 desc=out14
channel id=fb0   desc=fb0  events=on,off
channel id=fb1   desc=fb1  events=on,off
channel id=fb2   desc=fb2  events=on,off
channel id=fb3   desc=fb3  events=on,off
channel id=fb4   desc=fb4  events=on,off
channel id=fb5   desc=fb5  events=on,off
channel id=fb6   desc=fb6  events=on,off
channel id=fb7   desc=fb7  events=on,off
channel id=fb8   desc=fb8  events=on,off
channel id=fb9   desc=fb9  events=on,off
channel id=fb10  desc=fb10 events=on,off
channel id=fb11  desc=fb11 events=on,off
channel id=fb12  desc=fb12 events=on,off
channel id=fb13  desc=fb13 events=on,off
channel id=fb14  desc=fb14 events=on,off;

module  id=mfm_funk2.<addr + 7>
channel id=in0   desc=in0  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in1   desc=in1  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in2   desc=in2  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in3   desc=in3  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in4   desc=in4  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in5   desc=in5  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in6   desc=in6  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in7   desc=in7  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in8   desc=in8  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in9   desc=in9  events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in10  desc=in10 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in11  desc=in11 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in12  desc=in12 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in13  desc=in13 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=in14  desc=in14 events=Oingt0,Iingt0,Oout,Iout,Ooutlt1,Oingt1,Ooutgt1,Oingt2,Ioutlt1,Iingt1,Ioutgt1,Iingt2
channel id=out0  desc=out0
channel id=out1  desc=out1
channel id=out2  desc=out2
channel id=out3  desc=out3
channel id=out4  desc=out4
channel id=out5  desc=out5
channel id=out6  desc=out6
channel id=out7  desc=out7
channel id=out8  desc=out8
channel id=out9  desc=out9
channel id=out10 desc=out10
channel id=out11 desc=out11
channel id=out12 desc=out12
channel id=out13 desc=out13
channel id=out14 desc=out14
channel id=fb0   desc=fb0  events=on,off
channel id=fb1   desc=fb1  events=on,off
channel id=fb2   desc=fb2  events=on,off
channel id=fb3   desc=fb3  events=on,off
channel id=fb4   desc=fb4  events=on,off
channel id=fb5   desc=fb5  events=on,off
channel id=fb6   desc=fb6  events=on,off
channel id=fb7   desc=fb7  events=on,off
channel id=fb8   desc=fb8  events=on,off
channel id=fb9   desc=fb9  events=on,off
channel id=fb10  desc=fb10 events=on,off
channel id=fb11  desc=fb11 events=on,off
channel id=fb12  desc=fb12 events=on,off
channel id=fb13  desc=fb13 events=on,off
channel id=fb14  desc=fb14 events=on,off;
```
