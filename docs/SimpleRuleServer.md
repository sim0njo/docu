description: Simple Rule Server explained. 

## Intro
The Simple Rule Server is a lightweight daemon to accomplish automation tasks and which is integrated into the [Phc2Mqtt](../phc2mqtt) project,
we will furthermore refer to it as **SRSD**.

Operational behaviour of the SRSD is determined by a set of plain text rules stored in a file (rule-file)
which is loaded when the SRSD starts.
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
Rules are expressed in a simplified JSON format, removing redundant decoration, and are stored in a rule file. 

Strings that contain ';:{}[]"\', a space, a tab, a carriage-return (CR) or line-feed (LF) need enclosing double-quotes, 
additionally the double-quote/backslash/tab/CR/LF need to be escaped as '\"', '\\\\', '\t', '\r' and '\n' respectively. 

If a string does not contain above characters then it does not require enclosing double-quotes,
but they are allowed at the user's discretion.

A space/tab/CR/LF are considered white-space and are skipped, unless used inside a quoted string.

The pipe ('|') and ampersand ('&') are used as special separator characters inside a data-filter and cannot be used in a string.

Comments can be specified at any position on a line by means of a semicolon, all following characters upto the end-of-line will be skipped
unless used inside a quoted string.

Keywords and delimiters (between single quotes) in rules are case-insensitive, actual data strings are case-sensitive.

```ini
rules          ::= 'rules:{' *[<rule> | <comment>] '}'

comment        ::= ';' [<string>]

rule           ::= <init-rule> | <exit-rule> | <state-rule> | <action-rule>

init-rule      ::= 'init:{' *<action> '}'

exit-rule      ::= 'exit:{' *<action> '}'

state-rule     ::= 'state:{' 
                     'when:{' *<event> '}' 
                    ['refine:{' [<modifier>':'<string>]['keys:'<key-filters>]['math:'<term>] '}']
                   '}'
  modifier     ::= 'infix' | 'prefix' | 'name'
  term         ::= <oper> (<number> | <term>)
  oper         ::= '+-*/%&|^'

action-rule    ::= 'action:{'
                     'when:{' *<event> '}' 'then:{' *<action> '}' 
                   '}'

action-rule    ::= 'action:{'
                     'when:{' *<event> '}'
                     *('and:{' *<cond> '}' 'then:{' *<action> '}' )
                      ['else:{' *<action> '}' ]
                   '}'

                   
event          ::= <source>':{' 'topic:'<topic-filter> ['data:'<data-filter>] '}'
  source       ::= 'clock' | 'http' | 'mqtt' | 'stmd' | 'system' | 'timer'

cond           ::= <source>':{' 'topic:'<string> <oper>':'(<string>|<number>) '}'
  source       ::= 'clock' | 'state' | 'timer'
  oper         ::= 'eq' | 'ne' | 'lt' | 'gt' | 'le' | 'ge'

action         ::= <clock-action> | <mqtt-action> | <stmd-action> | <state-action> | <timer-action>
  clock-action ::= 'clock:{' 'topic:'<string> 'days:'<days> 
                           ['on:"' <0-23>:<0-59> '"' 'off:"' <0-23>:<0-59> '"'] '}'
    days       ::= sum of selected days: sun=1,mon=2,tue=4,wed=8,thu=16,fri=32,sat=64
  mqtt-action  ::= 'mqtt:{'  'topic:'<string> 'data:'<string> '}'
  stmd-action  ::= 'stmd:{'  'ccmd:'<phc-cmd> *[';'<phc-cmd>] '}'
  state-action ::= 'state:{' 'topic:'<string> 'value:'(<string>|<number>) '}'
  timer-action ::= 'timer:{' 'topic:'<string> 'seconds:'0-4294967295 '}'

key-filters    ::= <key-filter> *[','<key-filter>]
key-filter     ::= <string> | '?' | '#' | '*')
topic-filter   ::= <string> | '?' | '#' | '*')
  ?            ::= any character (1)
  #            ::= 1 or more numerical digits
  *            ::= any characters from this point on

data-filter    ::= <OR-pattern>
  OR-pattern   ::= <AND-pattern> *['|' <AND-pattern>]
  AND-pattern  ::= <string> *['&' <string>]

phc-cmd        ::= refer to the PHC Command Reference
number         ::= ['-+'] *'0-9' [ '.' *'0-9']
string         ::= ['"'] *<any-character> ['"']
```

## Internal Objects
SRSD knows a number of internal objects,  
they do not exist when SRSD starts but must be created/updated at runtime via an [action rule](#action-rule) or a [state rule](#state_rule).

These objects are persistent as long as SRSD is running, when the SRSD stops their values are lost.

Each internal object has 3 attributes:  
- type   : can be clock, state or timer  
- topic  : a meaningful string describing the nature of the object  
- value  : the value of the object, this can be a string or a number with a maximum of 34 characters

Each object can be referenced by type + topic in a [condition](#conditions) 
where it's value is compared to a condition-value to determine the flow of actions.

When a non-existing object is referred in a condition,
an temporary object will be created with default values depending on the object type, see below.

### Clock Object
A clock object can be used to perform periodic actions by specifying an on and off time and the days to apply.
It's default value is "off" (0).

You can use it in an action, refer to [Clock Action](#clock-action)

It will generate an event when it turns on or off, refer to [Clock Event](#clock-event)

You can use it's on/off value in a [Clock Condition](#clock-condition), either as number or as string

### State Object
A state object can be used to store runtime data for later use. It's default value is "" (0).

State objects can be created/updated implicitly with a [State Rule](#state-rule) 
or explicitly with a [State Action](#state-action)

A state object cannot generate any events.

You can use it's value in a condition, refer to [State Condition](#state-condition)

### Timer Object
A timer object can be used to monitor the presence or absence of an event (dead man's switch), 
it's unit of time is expressed in seconds. It's default value is "off" (0).

You can use it in an action, refer to [Timer Action](#timer-action)

It will generate an event when it starts/stops/expires, refer to [Timer Event](#timer-event)

You can use it's on/off value in a condition, refer to [Timer Condition](#timer-condition)

## Events
In general a rule is triggered by an event (unless otherwise stated) 
which has 2 parts of information that uniquely define the event: topic and data. Each part has it's characteristic formats.

### Topic Formats
#### Full
Fully identifies the object that generated the event, typically physical module + endpoint/channel:
```
topic:sta/omd.0.out0
```

#### Partial
Partially identifies part of the object that generated the event, typically the physical module:
```
topic:sta/omd.0
```

#### General
Does not identify the object that generated the event, it is a placeholder for topic-filtering:
```
topic:tzb/tele/SENSOR
```

### Data Formats
#### Flat
Has no formatting and is typically combined with a full-topic (equals xPhcLogd compatible STMD reporting format):
```
flat-format ::= <string> | <number>
  string    ::= *(a-z A-Z 0-9 _-/.)
  number    ::= integer | floating
```
Examples
```
data=outlt1
data=0
data=-25.9
```

#### JSON
Follows the JSON syntax rules, allows multiple endpoints/attributes to be reported in key:value pairs:
```
json-format    ::= '{' <key> ':' <value> *[',' <key> ':' <value>] '}'
  key          ::= <string>
  value        ::= <string> | <number> | <object> | <array> | <boolean> | <null>
  string       ::= '"' *(a-z A-Z 0-9 _-/.) '"'
  number       ::= integer | floating
  object       ::= { <key> : <value> *[, <key> : <value>] }
  array        ::= [ <value> *[, <value>] ]
  boolean      ::= false | true
  null         ::= null
```
Examples
```
data={"out0":1,"out1":1}
data={"Name":"living/id3","MultiInValue":1,"Click":"single","Endpoint":2,"LinkQuality":107}
```

#### Text
Similar to JSON format but without the syntax decoration, allows multiple endpoints/attributes to be reported in key:value pairs:
```
text-format ::= <key> [':' <value>] *[',' <key> [':' <value>]]
  key       ::= <string>
  value     ::= <string> | <number>
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
when:{clock:{topic:daily data:on}}
when:{clock:{topic:daily data:off}}
```

#### HTTP Event
External source, the result of an incoming HTTP request on the module's webserver, 
topic and data are contained in the HTTP query-string:
```
http-request ::= http://<module-ip-address>/srsd?<query-string>
query-string ::= <topic> ["&"<data>]
```

```
when:{http:{topic:ccmd data:imd.0.in0.outlt1}}
```
The HTTP request that meets above event definition would be:
```
http://<module-ip-address>/srsd?ccmd&imd.0.in0.outlt1
```
#### MQTT Event
External source, the result of the MQTT client receiving an incoming publish message that 
matches any of the configured SRSD RX-Topic-Prefixes.
Refer to [Configure Simple Rule Server](../phc2mqtt/#configure-simple-rule-server) in the Phc2Mqtt manual:
```
when:{mqtt:{topic:tzb/tele/SENSOR data:"Click\":\"single&Endpoint\":2"}}
```

#### STMD Event
External source, generated by the STMD reporting function, topic and data format depend on the Reporting Format/Data Format:
```
when:{stmd:{topic:evt/imd.0.in0 data:outlt1}}
when:{stmd:{topic:evt/imd.0 data:"in0:outlt1"}}
when:{stmd:{topic:evt/imd.0 data:"in0\":\"outlt1"}}
```

#### System Event
External source, will generate predefined events from the Phc2Mqtt module's sub-systems:
```
when:{system:{topic:appl data:halting     }} ; module is going to halt
when:{system:{topic:appl data:rebooting   }} ; module is going to reboot
when:{system:{topic:mqtt data:connected   }} ; MQTT client connected to broker
when:{system:{topic:mqtt data:disconnected}} ; MQTT client disconnected from broker
when:{system:{topic:stmd data:started     }} ; STM daemon started
when:{system:{topic:stmd data:stopped     }} ; STM daemon stopped
when:{system:{topic:wifi data:connected   }} ; Wifi client connected to AP
when:{system:{topic:wifi data:disconnected}} ; Wifi client disconnected from AP
```
The reason of existence for the system events comes from the startup order of the module's sub-systems. 
By default the SRSD will be started early during module boot. 
At that moment other sub-systems are not yet started and thus cannot execute actions from init rules.

By allowing SRSD to monitor when a given sub-system becomes active, you can take a matching action. 
For instance, you want to know when the MQTT client is connected to the broker so you can publish an 'online' message.

#### Timer Event
Internal source, generating an event when the specified timer turns on/off or expires:
```
when:{timer:{topic:watchdog data:on}}
when:{timer:{topic:watchdog data:off}}
when:{timer:{topic:watchdog data:expired}}
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
rule      : state:{ mqtt:{topic:tzb/tele/SENSOR data:"Name\":\"outside/lumi_nw"}
                    refine:{prefix:lumi keys:Illuminance} }
result    : state-topic=lumi/Illuminance state-value=24699
```
```
STMD-event: sta/omd.0 = out0:1,out4:0,out7:1
rule      : state:{ stmd:{topic:omd.# keys:out#} }
result    : state-topic=omd.0.out0 state-value=1
            state-topic=omd.0.out4 state-value=0
            state-topic=omd.0.out7 state-value=1
```
```
MQTT-event: tzb/tele/SENSOR = {"Name":"test/id1","MultiInValue":1,"Click":"single","Endpoint":1,"LinkQuality":139}
rule      : when:{ mqtt:{topic:tzb/tele/SENSOR data:"Name\":\"test/id1&Click\":\"single&Endpoint\":1|Name\":\"test/id1&Click\":\"single&Endpoint\":2"}}
```
```
MQTT-event: p2m/evt/imw.8 = in0:ingt0
MQTT-event: p2m/evt/imw.8 = in1:ingt0
rule      : when:{ mqtt:{topic:p2m/evt/imw.8 data:"in0:ingt0|in1:ingt0"}}
```
```
MQTT-event: test/parent.2/child.0  = value0:1
MQTT-event: test/parent.31/child.0 = value0:1
rule      : when:{ mqtt:{topic:test/parent.#/child.0 data:"value0:1"}}
```

## Conditions
In addition to event filtering we can add one or more conditions to an action rule to determine a specific flow of actions
in an **and**/**then**/**else** scheme, where each condition compares an internal object's value to a predefined value.

When a condition refers a non-existing internal object, SRSD will use a temporary object with a default value. 
Refer to [Internal Objects](#internal-objects) for default values for each type of object.

A condition compares the referenced object's value with the condition-value using an operator,
when both values are a number (integer or floating) they will be compared numerical otherwise as text.

The result of a condition will always be **true** or **false**, when multiple conditions are present then the
result of each condition is AND-ed. In that case the global result will be true if all conditions are true.

### Clock Condition
Compares the actual state of a clock, following formats are supported:
```ini
and:{clock:{topic:daily eq:on}} ; same as 1
and:{clock:{topic:daily ne:off}}; same as 0
and:{clock:{topic:daily eq:1}}  ; same as on
and:{clock:{topic:daily ne:0}}  ; same as off
```

### State Condition
Compare the actual value of a state-object, can be specified either as a string or a number:
```ini
and:{state:{topic:weather     eq:sunny}}
and:{state:{topic:temperature lt:21.5}}
```

### Timer Condition
Compares the actual state of a timer, following formats are supported:
```ini
and:{timer:{topic:watchdog eq:on}} ; same as 1
and:{timer:{topic:watchdog eq:off}}; same as 0
and:{timer:{topic:watchdog eq:1}}  ; same as on
and:{timer:{topic:watchdog eq:0}}  ; same as off
```

Examples:
```
object    : state:{topic:weather value:rainy}
condition : and:{state:{topic:weather eq:sunny}}
result    : false
```
```
object    : state:{topic;weather value:rainy}
condition : and:{state:{topic:weather lt:sunny}}
result    : true
```
```
object    : state:{topic:weather value:rainy}
condition : and:{state:{topic:weather lt:sunny} 
                 state:{topic:weather eq:rainday}}
result    : false
```
```
object    : state:{topic:temperature value:25.0}
condition : and:{state:{topic:temperature eq:25}}
result    : true
```
One or more **and** conditions can be specified and when evaluating to **true** 
SRSD will execute all following **then** actions after which the rule ends:
```
when:{ *<event> }
*[and:{ *<cond> } then:{ *<action> } ]
```
This **and**/**then** pattern can be repeated with the same behaviour explained above.

When all previous conditions were not met then SRSD will execute all **else** actions, if present:
```
*[else <action>]
```

## Actions
### Clock Action
With this action you can stop a clock by setting days to 0, 
or start it by setting days to the or-ed value of each required day and specifying the on and off time:
```
then:{clock:{topic:daily days:0}}
then:{clock:{topic:daily days:127 on:"7:00" off:"22:00"}}
```

### MQTT Action
With this action you can send an MQTT publish message via the module's MQTT client, with the specified topic and data.
```
then:{mqtt:{topic:tzb/cmnd/ZbSend data:"{\"device\":\"office/od4\",\"send\":{\"power\":\"toggle\"}}"}}
then:{mqtt:{topic:m2p/cmd/ccmd data:"omd.0.out0.toggle;omd.4.out7.toggle"}}
then:{mqtt:{topic:m2p/cmd/ccmd data:omd.0.out0.ontimed.30}}
```

### STMD Action
Send a list of phc-cmd's (ccmd) to the module's STMD sub-system, which will handle the commands depending on it's Operating Mode.
```
then:{stmd:{ccmd:"jrm.1.out0.stop.0;jrm.1.out0.delayedup.0.0.5.150"}}
```

### State Action
With this action you explicitly create or update an internal state-object that you can use in a condition. The state-object 
value can store upto 34 characters of string data, any longer data is truncated.
```
then:{state:{topic:weather value:sunny}}
then:{state:{topic:temperature value:21.6}}
```

### Timer Action
With this action you can stop a timer by setting seconds to 0, or start it by setting seconds to a non-zero value:
```
then:{timer:{topic:watchdog seconds:0}}
then:{timer:{topic:watchdog seconds:60}}
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
init:{
 state:{topic:sta/daynight value:day}
 mqtt :{topic:sta/srsd     data:online}
 stmd :{ccmd:"imw.0.led4.blink;omd.0.out0.on"}
 timer:{topic:watchdog     seconds:60}
 clock:{topic:daily        days:127 on:"7:00" off:"22:00"}
}
```

### Exit Rule
When the SRSD stops it will execute all exit rules without the need for an event to trigger the rule.

An exit rule executes any action but note that some actions make no sense (state/timer/clock) as the SRSD will stop any activity.
```
exit:{
 state:{topic:sta/daynight value:none}
 mqtt :{topic:sta/srsd     data:offline}
 stmd :{ccmd:"imw.0.led4.off;omd.0.out0.off"}
 timer:{topic:watchdog     seconds:0}
 clock:{topic:daily        days:0}
}
```

### State Rule
State rules are triggered by incoming events when matching the topic-filter and optional data-filter.

Their purpose is to extract information from the event-topic/event-data and store that into one or more state-objects for later reference.

A state-object's value can store upto 34 characters of string data, any longer data is truncated.

The simplest form of a state rule will use the event-topic as state-object topic and copy the event-data to the state-object's value.

To accomplish more complex transformations you can refine the rule with the **&lt;modifier>** and/or **keys** parameters. 
Depending on their combination the resulting state-objects will have a different topic and value as specified below:
```
modifier | keys | state-object-topic             | state-object-value
---------+------+--------------------------------+--------------------
-        | -    | <evt-topic>                    | <evt-data>
-        | x    | <evt-topic>/<key-name>         | <key-value>
infix    | -    | <evt-topic>/<infix>            | <evt-data>
infix    | x    | <evt-topic>/<infix>/<key-name> | <key-value>
prefix   | -    | <prefix>                       | <evt-data>
prefix   | x    | <prefix>/<key-name>            | <key-value>
name     | -    | <name>                         | <evt-data>
name     | x    | <name>                         | <key-value>
```

####keys
Events with a full topic typically have flat data associated with it, 
to handle this we do not use the &lt;modifier> nor &lt;keys> parameters.

The resulting state-object's topic will be equal to the event's topic, and it's value equal to the event's data.
This rule format covers the xPhcLogd compatible STMD reporting format.

All other events with a partial or general topic will typically have text or JSON formated data with key/value pairs,
the SRSD will parse them as follows.

Text format data:
The event data is first split into key/value pairs with a comma as separator, then split into key-name and key-value with a colon as separator:
```
Event data : out0:1,out1:0,out2:0,out3:0,out4:0,out5:1,out6:0,out7:0
Parsed as  : key-name=out0 key-value=1
             key-name=out1 key-value=0
             key-name=out2 key-value=0
             key-name=out3 key-value=0
             key-name=out4 key-value=0
             key-name=out5 key-value=1
             key-name=out6 key-value=0
             key-name=out7 key-value=0
```
JSON format data:
The event data is parsed following the JSON specification:
```
Event data : {"Device":"0x73C9","Name":"outside/lumi_nw","Illuminance":24699,"Endpoint":1}
Parsed as  : key-name=Device      key-value=0x73C9
             key-name=Name        key-value=outside/lumi_nw
             key-name=Illuminance key-value=24699
             key-name=Endpoint    key-value=1
```
For each parsed key present in the **keys** parameter, SRSD will create/update a state-object to store the key-value.

####examples
```
event       : topic=sta/omd.0.out0 data=1
rule        : state:{
               stmd:{topic:sta/omd.0.out0}
              }
state-object: topic=sta/omd.0.out0 value=1
```
```
event       : topic=evt/imd.0.in0 data=outlt1
rule        : state:{
               stmd:{topic:evt/imd.0.in0}
              }
state-object: topic=evt/imd.0.in0 value=outlt1
```
```
event       : topic=sta/omd.0 data=out0:1,out4:0,out7:1
rule        : state:{
               stmd:{topic:sta/omd.0 keys:out#}
              }
state-object: topic=sta/omd.0.out0 value=1
              topic=sta/omd.0.out4 value=0
              topic=sta/omd.0.out7 value=1
```
```
event       : topic=tzb/tele/SENSOR data={"Device":"0x73C9","Name":"outside/lumi_nw","Illuminance":24699,"Endpoint":1}
rule        : state:{
               mqtt:{topic:tzb/tele/SENSOR data:outside/lumi_nw} 
               refine:{infix:lumi keys:Illuminance}
              }
state-object: topic=tzb/tele/SENSOR/lumi/Illuminance value=24699
```
```
event       : topic=tzb/tele/SENSOR data={"Device":"0x73C9","Name":"outside/lumi_nw","Illuminance":24699,"Endpoint":1}
rule        : state:{
               mqtt:{topic:tzb/tele/SENSOR data:outside/lumi_nw} 
               refine:{prefix:lumi keys:Illuminance,Endpoint}
              }
state-object: topic=lumi/Illuminance value=24699
              topic=lumi/Endpoint value=1
```
```
event       : topic=sta/omd.0 data=out0:1
rule        : state:{
               stmd topic:sta/omd.0 data:out0} 
               refine:{keys:out0 name:bathroom/light}
              }
state-object: topic=bathroom/light value=1
```

### Action Rule
Action rules are triggered by an incoming event when matching the topic-filter and optional data-filter, 
see [event filtering](#event-filtering) for details.

Additional [conditions](#conditions) may be defined that determine the flow of actions executed.


##Rule Samples

###Samples for Text or JSON report format

!!! warning 
    These sample rules expect the [STMD Reporting](../phc2mqtt/#configure-stmd-reporting) Format to be 'Text' or 'JSON'
    and Data Format to be 'Binary'.

Simple controlling one or more outputs with one or more buttons:
```
action:{
 when:{
  ; clicking on a PHC button will trigger
  stmd:{ topic:evt/imw.13 data:"in4:outlt1" }

  ; or clicking on an Aqara Opple 3 band Zigbee button
  mqtt:{ topic:tzb/tele/SENSOR data:"Name\":\"aqara/inputs&Click\":\"single&Endpoint\":4" }
 }; when
 
 then:{
  ; toggle a PHC output
  stmd:{ ccmd:omd.4.out0.toggle }
  
  ; send multiple commands, note the double quotes are needed for the ; between the commands
  stmd:{ ccmd:"omd.4.out0.toggle;omd.4.out3.ontimed.5" }

  ; toggle an IKEA Zigbee outlet
  mqtt:{ topic:tzb/cmnd/ZbSend data:"{\"device\":\"ikea/outlet\",\"send\":{\"power\":\"toggle\"}}" }
 }; then
}; action
```

###Samples for xPhcLogd report format

!!! warning 
    These sample rules expect the [STMD Reporting](../phc2mqtt/#configure-stmd-reporting) Format to be 'xPhcLogd compatible'
    and Data Format to be 'Binary'.

Simple controlling one or more outputs with one or more buttons:
```
action:{
 when:{
  ; clicking on a PHC button will trigger
  stmd:{ topic:evt/imw.13.in4 data:outlt1 }

  ; or clicking on an Aqara Opple 3 band Zigbee button
  mqtt:{ topic:tzb/tele/SENSOR data:"Name\":\"aqara/inputs&Click\":\"single&Endpoint\":4" }
 }; when
 
 then:{
  ; toggle a PHC output
  stmd:{ ccmd:omd.4.out0.toggle }

  ; toggle an IKEA Zigbee outlet
  mqtt:{ topic:tzb/cmnd/ZbSend data:"{\"device\":\"ikea/outlet\",\"send\":{\"power\":\"toggle\"}}" }
 }; then
}; action
```

Controlling a PHC shutter with 2 PHC buttons:
```
action:{
 ; stop the shutter
 when:{
  stmd:{ topic:evt/imw.11.in0 data:ingt0 }
  stmd:{ topic:evt/imw.11.in1 data:ingt0 }
 }; when
 
 then:{
  stmd:{ ccmd:jrm.5.out2.stop.0 }
 }; then
}; action

action:{
 ; lower the shutter 15 seconds then lift 2 seconds to give some openings
 when:{
  stmd:{ topic:evt/imw.11.in0 data:ingt1 }
 };when
 then:{
  stmd:{ ccmd:jrm.5.out2.delayeddowntip.0.0.1.150.20 }
 }; then
}; action

action:{
 ; raise the shutter 15 seconds all the way up
 when:{
  stmd:{ topic:evt/imw.11.in1 data:ingt1 }
 };when
 then:{
  stmd:{ ccmd:jrm.5.out2.delayedup.0.0.1.150 }
 }; then
}; action
```

Using a Zigbee humidity sensor to drive a bathroom ventilator:
```
init:{
 ; initialise the fan state to on
 state:{ topic:bathroom/fan value:on }
 stmd :{ ccmd:omd.2.out5.on }
}; init

state:{
 ; collect runtime sensor data
 mqtt:{ topic:tzb/tele/SENSOR data:bathroom/sensor }
 refine:{ prefix:bathroom/sensor keys:Temperature,Humidity}
}; state

action:{
 ; let new humidity reading trigger the rule
 when:{
  mqtt:{ topic:tzb/tele/SENSOR data:bathroom/sensor&Humidity" }
 }; when

 ; if fan off and humidity more than 75% then turn fan on
 and:{
  state:{ topic:bathroom/fan eq:off }
  state:{ topic:bathroom/sensor/Humidity gt:75 }
 }; if
 
 then:{
  state:{ topic:bathroom/fan value:on }
  stmd :{ ccmd:omd.2.out5.on }
 }; then

 ; if fan on and humidity less than 70% then turn fan off
 and:{
  state:{ topic:bathroom/fan eq:on }
  state:{ topic:bathroom/sensor/Humidity lt:70 }
 }; if
 
 then:{
  state:{ topic:bathroom/fan value:off }
  stmd :{ ccmd:omd.2.out5.off }
 }; then
}; action
```

Automatic night lights with a Zigbee light sensor:
```
init:{
 ; initialise to night as the sensor will send updates during day
 state:{ topic:daynight value:night }
 stmd :{ ccmd:"omd.0.out4.on;omd.4.out2.on" }
}; init

state:{
 ; collect runtime sensor data
 mqtt:{ topic:tzb/tele/SENSOR data:outside/lumi_nw }
 refine:{ prefix:outside keys:Illuminance }
}; state

action:{
 ; let new illuminance reading trigger the rule
 when:{
  mqtt:{ topic:tzb/tele/SENSOR data:outside/lumi_nw&Illuminance" }
 }; when

 ; going from day to night -> turn on lights
 and:{
  state:{ topic:daynight eq:day }
  state:{ topic:outside/Illuminance lt:5000 }
 }; if
 
 then:{
  state:{ topic:daynight value:night }
  stmd :{ ccmd:"omd.0.out4.on;omd.4.out2.on" }
 }; then

 ; going from night to day -> turn off lights
 and:{
  state:{ topic:daynight eq:night }
  state:{ topic:outside/Illuminance gt:5500 }
 }; if
 
 then:{
  state:{ topic:daynight value:day }
  stmd :{ ccmd:"omd.0.out4.off;omd.4.out2.off" }
 }; then
}; action
```

Sample how to step through a series of state values pressing a button:
```
; set initial color
init:{ state:{ topic:mycolor value:white } }

action:{
 ; each time you press/release a button
 when:{ stmd:{ topic:evt/imw.8.in0 data:outlt1 } }

 and: { state:{ topic:mycolor eq:white } }
 then:{ state:{ topic:mycolor value:blue } }

 and: { state:{ topic:mycolor eq:blue } }
 then:{ state:{ topic:mycolor value:green } }

 and: { state:{ topic:mycolor eq:green } }
 then:{ state:{ topic:mycolor value:cyan } }

 and: { state:{ topic:mycolor eq:cyan } }
 then:{ state:{ topic:mycolor value:red } }

 and: { state:{ topic:mycolor eq:red } }
 then:{ state:{ topic:mycolor value:purple } }

 and: { state:{ topic:mycolor eq:purple } }
 then:{ state:{ topic:mycolor value:yellow } }
 
 else:{ state:{ topic:mycolor value:white } }
}; action
```
