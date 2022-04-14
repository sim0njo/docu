description: Simple Rule Server explained.

## Intro
The Simple Rule Server is a lightweight daemon to accomplish automation tasks and which is integrated into the [Phc2Mqtt](../phc2mqtt) project,
we will furthermore refer to it as **SRSD**.

Operational behaviour of the SRSD is determined by a set of plain text rules stored in a file (rule-file) which is loaded when the SRSD starts.
[Configure Simple Rule Server](../phc2mqtt/#configure-simple-rule-server) describes how this rule-file is provisioned in the Phc2Mqtt project.

In general above rules are triggered by an event (unless otherwise stated), 
these can come from different sources like an MQTT client, HTTP server, a clock, a timer or the hosting application
(in casu Phc2Mqtt provides system and STMD events).

Some rules are able to store part of an event's content or a fixed value into an internal piece of memory called a state-object, 
which can be used in a condition.

A condition compares the value of a state-object to a fixed value to determine the flow of actions.

Finally, the purpose of SRSD is to execute actions to realise the automation tasks, 
this can be sending an MQTT message, execute a PHC command, starting or stopping a clock or timer, ...

## Rule Syntax
Rules are expressed in a simplified JSON format, removing redundant decoration, and stored in a rule file.

Strings that only contain a-z A-Z 0-9, underscore, hyphen, forward slash or a period don't required surrounding double-quotes, although
they are allowed. 

If a string contains other characters than described above, it needs to be enclosed in double-quotes. If the string itself 
contains a double-quote then it needs to be escaped as \"


A rule can span 1 or more lines but the last line always ends on a semicolon (;).

Each line in a rule carries 1 functional block as shown in the Backus-Naur format below, the split over lines is intentional and strict.

A space character is used to separate rule fields and can thus not be used as data.

Keywords and delimiters (between double quotes) in rules are case-insensitive, actual data strings are case-sensitive.

```ini
rules          ::= rules:{ *[<rule> | <comment>] }

comment        ::= ; [<string>]

rule           ::= <init-rule> | <exit-rule> | <state-rule> | <action-rule>

init-rule      ::= init:{ <action> }

exit-rule      ::= exit:{ <action> }

state-rule     ::= state:{ <event> [<modifier>] [keys:<key-filter> *[<comma><key-filter>]]
  modifier     ::= infix:<string> | prefix:<string> | name:<string>

action-rule    ::= action:{ *when:{ <event> } *then:{ <action> } }

action-rule    ::= *when:{ <event> }  
                     if:{ <cond> }   *[and:{ <cond> } ]  
                       *then:{ <action> }  
                     *[elif:{ <cond> } *[and:{ <cond> } ]  
                       *then:{ <action> } ]  
                     *[else:{ <action> } ]

event          ::= src:<source> topic:<topic-filter> [data:<data-filter>]
  source       ::= clock | http | mqtt | stmd | system | timer

cond           ::= src:<source> topic:<topic-name> oper:<oper> value:<string>|<number>
  source       ::= clock | state | timer
  oper         ::= eq | ne | lt | gt | le | ge

action         ::= <clock-action> | <mqtt-action> | <stmd-action> | <state-action> | <timer-action>
  clock-action ::= dst:clock topic:<topic-name> days:<days> [onhour:0-23 onminute:0-59 offhour:0-23 offminute:0-59]
  mqtt-action  ::= dst:mqtt  topic:<topic-name> data:<string>
  stmd-action  ::= dst:stmd  ccmd:<phc-cmd> *[<semicolon><phc-cmd>]
  state-action ::= dst:state topic:<topic-name> value:<string>|<number>
  timer-action ::= dst:timer topic:<topic-name> seconds:0-4294967295
    days       ::= or-ed value of selected days: sun=1,mon=2,tue=4,wed=8,thu=16,fri=32,sat=64

topic-filter   ::= *('a-z' | 'A-Z' | '0-9' | '_-/.' | '?' | '#' | '*')
key-filter     ::= *('a-z' | 'A-Z' | '0-9' | '_-/.' | '?' | '#' | '*')
  ?            ::= any character (1)
  #            ::= 1 or more numerical digits
  *            ::= any characters from this point on

data-filter    ::= <OR-pattern>
  OR-pattern   ::= <AND-pattern> *[<pipe><AND-pattern>]
  AND-pattern  ::= <string> *[<ampersand><string>]

topic-name     ::= <basic-string>
phc-cmd        ::= refer to the PHC Command Reference
number         ::= *('0-9') [ <period> *('0-9')]
string         ::= <basic-string> | <quoted-string>
basic-string   ::= *('a-z' | 'A-Z' | '0-9' | '_-/.')
quoted-string  ::= <double-quote> *('a-z' | 'A-Z' | '0-9' | '_-/. ,|&\"':;{}[]') <double-quote>

period         ::= '.'
comma          ::= ','
semicolon      ::= ';'
pipe           ::= '|'
ampersand      ::= '&'
double-quote   ::= '"'
```

```ini
rules          ::= "<rules>"
                   *[<rule> | <comment>]
                   "</rules>"

comment        ::= ";" [<string>]

rule           ::= (<init rule> | <exit rule> | <state-rule> | <action-rule>) ";"

init rule      ::= "init" <action>

exit rule      ::= "exit" <action>

state-rule     ::= "state" <event> [<modifier>"="<string>] ["keys="<key-filter> *["," <key-filter>]]
  modifier     ::= "infix" | "prefix" | "name"

action-rule    ::= *("when" <event>)  
                     *("then" <action>)

action-rule    ::= *("when" <event>)  
                   "if" <cond>  
                     *["and" <cond>]  
                       *("then" <action>)  
                   *["elif" <cond>  
                     *["and" <cond>]  
                       *("then" <action>)]  
                   *["else" <action>]

event          ::= <source> "topic="<topic-filter> ["data="<data-filter>]
  source       ::= "<clock" | "<http" | "<mqtt" | "<stmd" | "<system" | "<timer"

cond           ::= <source> "topic="<topic-name> "oper="<oper> "value="<string>|<number>
  source       ::= "<clock" | "<state" | "<timer"
  oper         ::= "eq" | "ne" | "lt" | "gt" | "le" | "ge"

action         ::= <clock-action> | <mqtt-action> | <stmd-action> | <state-action> | <timer-action>
  clock-action ::= ">clock topic="<topic-name> "days="<days> ["onhour="0-23 "onminute="0-59 "offhour="0-23 "offminute="0-59]
  mqtt-action  ::= ">mqtt  topic="<topic-name> "data="<string>
  stmd-action  ::= ">stmd  ccmd="<phc-cmd> *[";" <phc-cmd>]
  state-action ::= ">state topic="<topic-name> "value="<string>|<number>
  timer-action ::= ">timer topic="<topic-name> "seconds="0-4294967295

topic-filter   ::= *("a-z" | "A-Z" | "0-9" | "_-/." | "?" | "#" | "*")
key-filter     ::= *("a-z" | "A-Z" | "0-9" | "_-/." | "?" | "#" | "*")
  ?            ::= any character (1)
  #            ::= 1 or more numerical digits
  *            ::= any characters from this point on

data-filter    ::= <OR-pattern>
  OR-pattern   ::= <AND-pattern> *["|" <AND-pattern>]
  AND-pattern  ::= <string> *["&" <string>]

topic-name     ::= *("a-z" | "A-Z" | "0-9" | "_-/.")
phc-cmd        ::= refer to the PHC Command Reference
number         ::= *("0-9") ["." *("0-9")]
string         ::= *("a-z" | "A-Z" | "0-9" | "_-/.,"':;{}")
days           ::= or-ed value of selected days: sun=1,mon=2,tue=4,wed=8,thu=16,fri=32,sat=64
```

## Internal Objects
The SRSD knows a number of internal objects, 
they do not exist when SRSD starts but must be created/updated at runtime via an [action rule](#action-rule) or a [state rule](#state_rule).

These objects are persistent as long as SRSD is running, when the SRSD stops their values are lost.

Each internal object has 2 public attributes:  
- topic  : a meaningful string describing the nature of the object  
- value  : the string value of the object

Each object can be referred by topic in a [condition](#conditions) where it's value is compared to a condition-value to determine the flow of actions.

When a non-existing object is referred in a condition,
an empty object will be used with default values depending on the object type, see below.

### Clock Object
A clock object can be used to perform periodic actions.
You can specify which days of the week it should be applied and what time to turn on and off the clock

You can use it in a clock action:
stop a clock by setting days to 0,
start a clock by settings days to the or-ed value of each required day and specifying the on and off time:
```
then >clock topic=daily days=0
then >clock topic=daily days=127 onhour=7 onminute=0 offhour=22 offminute=0
```
It will generate an event when it turns on or off:
```
when <clock topic=daily data=on
when <clock topic=daily data=off
```
You can use it in a condition, it's default value="off":
```ini
if   <clock topic=daily oper=eq value=on
and  <clock topic=daily oper=ne value=off
```
### State Object
A state object can be used to store runtime data for later use.

State objects can be created/updated implicitly with a [state rule](#state-rule).
```
state <mqtt topic=tzb/tele/SENSOR data=outside/lumi_nw prefix=lumi keys=Illuminance;
```

You can create/update a state object explicitly with a state action:
```
then >state topic=weather value=sunny
then >state topic=temperature value=20.9
```

A state object cannot generate any events.

You can use it in a condition, it's default value="":
```ini
if   <state topic=weather     oper=eq value=sunny
if   <state topic=temperature oper=lt value=21.5
```

### System Object
A system object is always present and cannot be created/updated as such. 
It is the link between the Phc2Mqtt module with it's sub-systems and SRSD.

It will generate following predefined events:
```
when <system topic=appl data=halting        ; module is going to halt
when <system topic=appl data=rebooting      ; module is going to reboot
when <system topic=mqtc data=connected      ; MQTT client connected to broker
when <system topic=mqtc data=disconnected   ; MQTT client disconnected from broker
when <system topic=stmd data=started        ; STM daemon started
when <system topic=stmd data=stopped        ; STM daemon stopped
when <system topic=wifi data=connected      ; Wifi client connected to AP
when <system topic=wifi data=disconnected   ; Wifi client disconnected from AP
```
The reason of existence for the system events comes from the startup order of the module's sub-systems. 
By default the SRSD will be started early during module boot. 
At that moment other sub-systems are not yet started and thus cannot execute actions from init rules.

By allowing SRSD to monitor when a given sub-system becomes active, you can take a matching action. 
For instance, you want to know when the MQTT client is connected to the broker so you can publish an 'online' message.

### Timer Object
A timer object can be used to monitor the presence or absence of an event (dead man's switch), it's unit of time is expressed in seconds.

You can use it in an action:
stop a timer by setting seconds to 0,
start it by setting seconds to a non-zero value:
```ini
then >timer topic=watchdog seconds=0
then >timer topic=watchdog seconds=60
```

It will generate an event when it expires:
```ini
when <timer topic=watchdog data=expired
```

You can use it in a condition, it's default value="off":
```ini
if   <timer topic=watchdog oper=eq value=on
if   <timer topic=watchdog oper=eq value=off
```

## Events
In general a rule is triggered by an event (unless otherwise stated) 
which has 2 parts of information that uniquely define the event: topic and data. Each part has it's characteristic formats.

### Topic Formats
#### Full
Fully identifies the object that generated the event, typically physical module + endpoint/channel:
```
topic=sta/omd.0.out0
```

#### Partial
Partially identifies part of the object that generated the event, typically the physical module:
```
topic=sta/omd.0
```

#### General
Does not identify the object that generated the event, it is a placeholder for topic-filtering:
```
topic=tzb/tele/SENSOR
```

### Data Formats
#### Flat
Has no formatting and is typically combined with a full-topic (equals xPhcLogd compatible STMD reporting format):
```
flat-format ::= <string> | <number>
  string    ::= *(<b>"a-z"</b> | "A-Z" | "0-9" | "_-/.")
  number    ::= integer | floating
```
Examples
```
data=outlt1
data=0
data=25.9
```

#### JSON
Follows the JSON syntax rules, allows multiple endpoints/attributes to be reported in key:value pairs:
```
json-format    ::= { <key-name> : <key-value> *[, <key-name> : <key-value>] }
  key-name     ::= <string>
  key-value    ::= <string> | <number> | <object> | <array> | <boolean> | <null>
  string       ::= <double-quote> *(a-z A-Z 0-9 _-/.) <double-quote>
  number       ::= integer | floating
  object       ::= { <key> : <value> *[, <key> : <value>] }
  array        ::= [ <value> *[, <value>] ]
  boolean      ::= false | true
  null         ::= null
  double-quote ::= "
```
Examples
```
data={"out0":1,"out1":1}
data={"Name":"living/id3","MultiInValue":1,"Click":"single","Endpoint":2,"LinkQuality":107}
```

#### Text
Similar to JSON format but without the syntax decoration, allows multiple endpoints/attributes to be reported in key:value pairs:
```
text-format ::= <key-name> [ : <key-value>] *[ , <key-name> [ : <key-value>]]
  key-name  ::= <string>
  key-value ::= <string> | <number>
  string    ::= *(a-z A-Z 0-9 _-/.)
  number    ::= integer | floating
```
Examples
```
data=out0:1,out1:1
```

### Event Sources
Events are generated by a source, these can be internal or external to the SRSD as explained here.

#### Clock Event
Internal source, generating an event when the specified clock turns on or off:
```
when <clock topic=daily data=on
when <clock topic=daily data=off
```

#### HTTP Event
External source, the result of an incoming HTTP request on the module's webserver, 
topic and data are contained in the HTTP query-string:
```
http-request ::= http://<module-ip-address>/srsd?<query-string>
query-string ::= <topic> ["&"<data>]
```

```
when <http topic=ccmd data=imd.0.in0.outlt1
```
The HTTP request that meets above event definition would be:
```
http://<module-ip-address>/srsd?ccmd&imd.0.in0.outlt1
```
#### MQTT Event
External source, the result of the MQTT client receiving an incoming publish message that matches any of the configured SRSD RX-topic-prefixes.
Refer to Configuring Simple Rule Server in the Phc2Mqtt manual:
```
when <mqtt topic=tzb/tele/SENSOR data=Click":"single&Endpoint":2
```

#### STMD Event
External source, generated by the STMD reporting function, topic and data format depend on the Reporting Mode:
```
when <stmd topic=evt/imd.0.in0 data=outlt1
when <stmd topic=evt/imd.0 data=in0:outlt1
when <stmd topic=evt/imd.0 data={"in0":"outlt1"}
```

#### System Event
External source, will generate predefined events from the Phc2Mqtt module's sub-systems:
```
when <system topic=appl data=halting        ; module is going to halt
when <system topic=appl data=rebooting      ; module is going to reboot
when <system topic=stmd data=started        ; STM daemon started
when <system topic=stmd data=stopped        ; STM daemon stopped
when <system topic=mqtt data=connected      ; MQTT client connected to broker
when <system topic=mqtt data=disconnected   ; MQTT client disconnected from broker
```

#### Timer Event
Internal source, generating an event when the specified timer expires:
```
when <timer topic=watchdog data=expired
```

### Event Filtering
For every incoming event the SRSD will iterate over the list of state/action-rules in the order they were defined.
If the topic-filter and optional data-filter match then the rule will be executed, otherwise it will be skipped.

#### Topic-filters
Are used to match the event topic and support wildcard characters:  
- A question-mark (<b>?</b>) means that the character in that position can be any character  
- A hashtag (<b>#</b>) means one or more numeric digits match  
- An asterisk (<b>*</b>) means all following characters are a match  
- All other characters (<b>0-9</b>,<b>a-z</b>,<b>A-Z</b>, <b>_-/.</b>) require a 1-on-1 case-sensitive match

#### Data-filters
Are used to match the event data and are composed from OR- and AND-patterns:  
- All strings in the AND-pattern must be present as-is (case-sensitive) in the event data for the AND-pattern to match  
- You can specify multiple AND-patterns combined in an OR-pattern, as soon as one AND-pattern matches, the data-filter matches

####Key-filters
Are used in state-rules to extract key:value pairs from text/JSON event data and support wildcard characters:  
- A <b>?</b> means that the character in that position can be any character  
- A <b>#</b> means one or more numeric digits match  
- An <b>*</b> means all following characters are a match  
- All other characters require a 1-on-1 case-sensitive match

Samples for illustration:
```
MQTT-event: tzb/tele/SENSOR = {"Name":"outside/lumi_nw","Illuminance":24699,"Endpoint":1}
rule      : state <mqtt topic=tzb/tele/SENSOR data=Name":"outside/lumi_nw prefix=lumi keys=Illuminance;
result    : state-topic=lumi/Illuminance state-value=24699
```
```
STMD-event: sta/omd.0 = out0:1,out4:0,out7:1
rule      : state <stmd topic=omd.# keys=out#;
result    : state-topic=omd.0.out0 state-value=1
            state-topic=omd.0.out4 state-value=0
            state-topic=omd.0.out7 state-value=1
```
```
MQTT-event: tzb/tele/SENSOR = {"Name":"test/id1","MultiInValue":1,"Click":"single","Endpoint":1,"LinkQuality":139}
rule      : when  <mqtt topic=tzb/tele/SENSOR data=Name":"test/id1&Click":"single&Endpoint":1|Name":"test/id1&Click":"single&Endpoint":2
```
```
MQTT-event: p2m/evt/imw.8 = in0:ingt0
MQTT-event: p2m/evt/imw.8 = in1:ingt0
rule      : when  <mqtt topic=p2m/evt/imw.8   data=in0:ingt0|in1:ingt0
```
```
MQTT-event: test/parent.2/child.0  = value0:1
MQTT-event: test/parent.31/child.0 = value0:1
rule      : when  <mqtt topic=test/parent.#/child.0 data=value0:1
```

## Conditions
In addition to event filtering we can add one or more conditions to an action rule to determine a specific flow of actions
in an **if**/**then**/**elif**/**else** scheme, where each condition compares an internal object's value to a predefined value.

When a condition refers a non-existing internal object, SRSD will use a temporary object with a default value. 
Refer to [Internal Objects](#internal-objects) for default values for each type of object.

A condition compares the referred object's value with the condition-value using an operator,
when both values are a number (integer or floating) they will be compared numerical otherwise as text.

The result of a condition will always be **true** or **false**, when multiple conditions are present then the
result of each condition is AND-ed:
```
object    : >state topic=weather value=rainy
condition : if  <state topic=weather oper=eq value=sunny
result    : false
```
```
object    : >state topic=weather value=rainy
condition : if  <state topic=weather oper=lt value=sunny
result    : true
```
```
object    : >state topic=weather value=rainy
condition : if  <state topic=weather oper=lt value=sunny
            and <state topic=weather oper=eq value=rainday
result    : false
```
```
object    : >state topic=temperature value=25.0
condition : if  <state topic=temperature oper=eq value=25
result    : true
```
Initially **if** and **and** conditions can be specified and when evaluating to **true** 
SRSD will execute all following **then** actions after which the rule ends:
```
when <event>
if <cond>
  *[and <cond>]
    *(then <action>)
```
Additional **elif** and **and** conditions can be specified, again when evaluating to **true** 
SRSD will execute all followng **then** actions after which the rule ends:
```
*[elif <cond>
  *[and <cond>]
    *(then <action>)]
```
When all previous conditions were not met then SRSD will execute all **else** actions, if present:
```
*[else <action>]
```
## Rules

### Init Rule
When the SRSD starts it will execute all init rules without the need for an event to trigger the rule.

An init rule executes any action but be aware that not all actions may execute as the targeted sub-system may not yet be active.

When the Phc2Mqtt module boots, the different sub-systems will start each in turn. Typically STMD first, then SRSD, Wifi, HTTP server and MQTT client.
In this case it will not be possible to send an MQTT message in an init rule. Checkout the [system events](#system-event) to handle this problem.

Ofcourse SRSD can also be started manually from the configuration page, at that moment the rest of the module is fully operational and
it will be possible to send an MQTT message.

```
init  >state topic=alias/sta/daynight value=day;
init  >mqtt  topic=alias/sta/srsd     data=online;
init  >stmd  ccmd=imw.0.led4.blink;omd.0.out0.on;
init  >timer topic=watchdog           seconds=60;
init  >clock topic=daily              days=127 onhour=7 onminute=0 offhour=22 offminute-0;
```

### Exit Rule
When the SRSD stops it will execute all exit rules without the need for an event to trigger the rule.

An exit rule executes any action but note that some actions make no sense (state/timer/clock) as the SRSD will stop any activity.
```
exit  >state topic=alias/sta/daynight value=none;
exit  >mqtt  topic=alias/sta/srsd     data=offline;
exit  >stmd  ccmd=imw.0.led4.off;omd.0.out0.off
exit  >timer topic=watchdog           seconds=0;
exit  >clock topic=daily              days=0;
```

### State Rule
State rules are triggered by incoming events when matching the &lt;topic-filter> and optional &lt;data-filter>.

Their purpose is to extract information from the event topic/data and store that into one or more state objects for later reference.

This is a complex operation because the event topic/data are highly variable with respect to content and format.

To accomplish this you can use the &lt;modifier> and &lt;keys> parameters, depending on their combination the resulting state-objects
will have a different name and value as specified in below table:
```
modifier | keys | state-object-name             | state-object-value
---------+------+-------------------------------+--------------------
-        | -    | <topic>                       | <data>
-        | x    | <topic>/<key-name>            | <key-value>
infix    | -    | <topic>/<infix>               | <data>
infix    | x    | <topic>/<infix>/<key-name>    | <key-value>
prefix   | -    | <prefix>                      | <data>
prefix   | x    | <prefix>/<key-name>           | <key-value>
name     | -    | <name>                        | <data>
name     | x    | <name>                        | <key-value>
```

####keys
Events with a full topic typically have flat data associated with it, 
to handle this we do not use the &lt;modifier> nor &lt;keys> parameters.

The resulting state-object's name will be equal to the event's topic, and a value equal to the event's data.
This rule format covers the xPhcLogd compatible STMD reporting format.

All other events with a partial or general topic will typically have text or JSON formated data with key/value pairs,
the SRSD will parse them as follows.

Text format data:
The event data is first split into key/value pairs with a comma as separator, then split into key and value with a colon as separator:
```
Event data : out0:1,out1:0,out2:0,out3:0,out4:0,out5:1,out6:0,out7:0
Parsed as  : key=out0 value=1
             key=out1 value=0
             key=out2 value=0
             key=out3 value=0
             key=out4 value=0
             key=out5 value=1
             key=out6 value=0
             key=out7 value=0
```
JSON format data:
The event data is parsed following the JSON specification:
```
Event data : {"Device":"0x73C9","Name":"outside/lumi_nw","Illuminance":24699,"Endpoint":1}
Parsed as  : key=Device      value=0x73C9
             key=Name        value=outside/lumi_nw
             key=Illuminance value=24699
             key=Endpoint    value=1
```
For each parsed key present in the **keys** parameter, SRSD will create/update a state-object to store the key-value.

####examples
```
event       : topic=sta/omd.0.out0 data=1
rule        : state <stmd topic=sta/omd.0.out0 etopic;
state-object: topic=sta/omd.0.out0 value=1
```
```
event       : topic=evt/imd.0.in0 data=outlt1
rule        : state <stmd topic=evt/imd.0.in0 etopic;
state-object: topic=evt/imd.0.in0 value=outlt1
```
```
event       : topic=sta/omd.0 data=out0:1,out4:0,out7:1
rule        : state <stmd topic=sta/omd.0 keys=out#;
state-object: topic=sta/omd.0.out0 value=1
              topic=sta/omd.0.out4 value=0
              topic=sta/omd.0.out7 value=1
```
```
event       : topic=tzb/tele/SENSOR data={"Device":"0x73C9","Name":"outside/lumi_nw","Illuminance":24699,"Endpoint":1}
rule        : state <mqtt topic=tzb/tele/SENSOR data=outside/lumi_nw infix=lumi keys=Illuminance;
state-object: topic=tzb/tele/SENSOR/lumi/Illuminance value=24699
```
```
event       : topic=tzb/tele/SENSOR data={"Device":"0x73C9","Name":"outside/lumi_nw","Illuminance":24699,"Endpoint":1}
rule        : state <mqtt topic=tzb/tele/SENSOR data=outside/lumi_nw prefix=lumi keys=Illuminance,Endpoint;
state-object: topic=lumi/Illuminance value=24699
              topic=lumi/Endpoint value=1
```
```
event       : topic=sta/omd.0 data=out0:1
rule        : state <stmd topic=sta/omd.0 data=out0 keys=out0 name=bathroom/light;
state-object: topic=bathroom/light value=1
```

### Action Rule
Action rules are triggered by an incoming event when matching the topic-filter and optional data-filter, 
see [event filtering](#event-filtering) for details.

Additional [conditions](#conditions) may be defined that determine the flow of actions executed.

#### Clock Action
Stop a clock by setting days to 0, start it by setting days to the or-ed value of each required day and specifying the on and off time:
```
then >clock topic=daily days=0
then >clock topic=daily days=127 onhour=7 onminute=0 offhour=22 offminute=0
```

#### MQTT Action
Send an MQTT publish message via the module's MQTT client, with the specified topic and data.
```
then >mqtt  topic=tzb/cmnd/ZbSend data={"device":"office/od4","send":{"power":"toggle"}};
then >mqtt  topic=m2p/cmd/ccmd data=omd.0.out0.toggle;omd.4.out7.toggle
```

#### STMD Action
Send a list of phc-cmd's (ccmd) to the module's STMD sub-system, which will handle the commands depending on it's Operating Mode.
```
then >stmd   ccmd=jrm.1.out0.stop.0;jrm.1.out0.delayedup.0.0.5.150;
```

#### State Action
Explicitly create or update an internal state-object.
```
then >state topic=weather value=sunny
then >state topic=temperature value=21.6
```

#### Timer Action
Stop a timer by setting seconds to 0, start it by setting seconds to a non-zero value:
```
then >timer topic=watchdog seconds=0
then >timer topic=watchdog seconds=60
```





##Rule Samples

!!! warning 
    These sample rules expect the [STMD Reporting](../phc2mqtt/#configure-stmd-reporting) Format to be 'xPhcLogd compatible'.

Simple controlling one or more outputs with one or more buttons:
```
; clicking on a PHC button will trigger
when  <stmd topic=evt/imw.13.in4 data=outlt1

; or clicking on an Aqara Opple 3 band Zigbee button
when  <mqtt topic=tzb/tele/SENSOR data="Name":"aqara/inputs"&"Click":"single"&"Endpoint":4

; toggle a PHC output
then  >stmd ccmd=omd.4.out0.toggle

; toggle an IKEA Zigbee outlet
then  >mqtt topic=tzb/cmnd/ZbSend data={"device":"ikea/outlet","send":{"power":"toggle"}};
```

Controlling a PHC shutter with 2 PHC buttons:
```
; stop the shutter
when  <stmd topic=evt/imw.11.in0 data=ingt0
when  <stmd topic=evt/imw.11.in1 data=ingt0
then  >stmd ccmd=jrm.5.out2.stop.0;

; lower the shutter 15 seconds then lift 2 seconds to give some openings
when  <stmd topic=evt/imw.11.in0 data=ingt1
then  >stmd ccmd=jrm.5.out2.delayeddowntip.0.0.1.150.20;

; raise the shutter 15 seconds all the way up
when  <stmd topic=evt/imw.11.in1 data=ingt1
then  >stmd ccmd=jrm.5.out2.up.0.0.150;
```

Using a Zigbee temperature/humidity sensor to drive a bathroom ventilator:
```
; initialise the fan state
init  >state topic=bathroom/fan value=off;

state <mqtt topic=tzb/tele/SENSOR data=bathroom/sensor prefix=bathroom/sensor keys=Temperature,Humidity;

; let new humidity reading trigger the rule
when  <mqtt topic=tzb/tele/SENSOR data=bathroom/sensor&Humidity":

; when off, check for more than 75% humidity to turn fan on
if    <state topic=bathroom/fan oper=eq value=off
and   <state topic=bathroom/sensor/Humidity oper=gt value=75
then  >state topic=bathroom/fan value=on
then  >stmd  ccmd=omd.2.out5.on

; when on, check for less than 70% humidity to turn fan off
elif  <state topic=bathroom/fan oper=eq value=on
and   <state topic=bathroom/sensor/Humidity oper=lt value=70
then  >state topic=bathroom/fan value=off
then  >stmd  ccmd=omd.2.out5.off;
```

Automatic night lights with a Zigbee light sensor:
```
; initialise to night as the sensor will send updates during day
init  >state topic=daynight value=night;

; extract the illuminance value into a state object
state <mqtt  topic=tzb/tele/SENSOR data=outside/lumi_nw prefix=outside keys=Illuminance;

; check each update coming in from the sensor
when  <mqtt  topic=tzb/tele/SENSOR data=outside/lumi_nw&Illuminance":

; going from day to night -> turn on lights
if    <state topic=daynight oper=eq value=day
and   <state topic=outside/Illuminance oper=lt value=5000
then  >state topic=daynight value=night
then  >stmd  ccmd=omd.0.out4.on;omd.4.out2.on

; going from night to day -> turn off lights
elif  <state topic=daynight oper=eq value=night
and   <state topic=outside/Illuminance oper=gt value=5500
then  >state topic=daynight value=day
then  >stmd  ccmd=omd.0.out4.off;omd.4.out2.off;
```

Sample how to step through a series of state values pressing a button:
```
; set initial color
init  >state topic=mycolor value=white;

; each time you press/release a button
when  <stmd  topic=evt/imw.8.in0 data=outlt1

if    <state topic=mycolor oper=eq value=white
then  >state topic=mycolor value=blue

elif  <state topic=mycolor oper=eq value=blue
then  >state topic=mycolor value=green

elif  <state topic=mycolor oper=eq value=green
then  >state topic=mycolor value=cyan

elif  <state topic=mycolor oper=eq value=cyan
then  >state topic=mycolor value=red

elif  <state topic=mycolor oper=eq value=red
then  >state topic=mycolor value=purple

elif  <state topic=mycolor oper=eq value=purple
then  >state topic=mycolor value=yellow

elif  <state topic=mycolor oper=eq value=yellow
then  >state topic=mycolor value=white

else  >state topic=mycolor value=white;

```
