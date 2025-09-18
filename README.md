
# BBjWebguiBridge

## Overview

`BBjWebguiBridge` is a plugin for BBj that provides a bridge to a Webswing Connector. It enables integration between BBj applications running in webswing and webforJ.

## Features

- Register and manage custom event callbacks between BBj and webforJ
- Send and receive custom events with payloads
- Simple API for integration with BBj applications

## Main Class

`BBjWebguiBridge` (see `BBjWebguiBridge.bbj`)

### Fields

- `listenerMap!` (`HashMap`): Stores event listeners by label
- `bridge!` (`WebguiBridge@`): The underlying Webgui bridge instance

### Methods

#### Constructor

```bbj
BBjWebguiBridge()
```
Initializes the bridge and listener map.

#### setCallback

```bbj
setCallback(BBjString callbackLabel$, CustomObject instance!, BBjString methodName$)
setCallback(String callbackLabel$, String methodName$)
```
Registers a callback for a custom event. Associates a listener with the event label and sets the callback in BBjAPI.

**Parameters:**
- `callbackLabel$`: The label for the callback/event
- `instance!`: (optional) The object instance for the callback
- `methodName$`: The method to invoke on the callback

#### removeCallback

```java
removeCallback(BBjString callbackLabel$)
```
Removes a previously registered callback and its listener.

#### postCustomEvent

```java
postCustomEvent(BBjString eventName!, String payload!)
```
Posts a custom event to the bridge and therefore the webswing connector, sending the specified payload.

## Usage Example

```java
use ::BBjWebguiBridge/BBjWebguiBridge.bbj::BBjWebguiBridge

bridge! = new BBjWebguiBridge()
bridge!.setCallback("onCustomEvent", instance!, "handleEvent")
bridge!.postCustomEvent("onCustomEvent", "{" + """data""" + ":" + """Hello World """ + "}")

handleEvent:
    json! = CAST(JsonObject, ev!.getObject())
    print json!
```

## Dependencies

- BASIS BBj
- BASIS WebguiBridge Java library (`lib/webgui-bridge-25.03-SNAPSHOT.jar`)

## Listeners

Default listener implementation: `listeners/BBjWebguiDefaultListener.bbj`