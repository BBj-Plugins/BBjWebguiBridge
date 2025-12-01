
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
postCustomEvent(BBjString eventName!)
```
Posts a custom event to the bridge and therefore the webswing connector.

```java
postCustomEvent(BBjString eventName!, JsonObject@ payload!)
```
Posts a custom event to the bridge and therefore the webswing connector, sending the specified payload.


## Listeners

### BBjWebguiDefaultListener

Default listener implementation: `listeners/BBjWebguiDefaultListener.bbj`

This class implements `WebguiListenerIF` and handles incoming events from the WebguiBridge.

### Methods

#### onEventTriggered(WebguiObjectIF@ webguiObject!)

Handles events triggered by the WebguiBridge.

**Parameters:**
- `webguiObject!`: `WebguiObjectIF@` object containing the event data

**Behavior:**
- Extracts the action name from the incoming event
- Extracts the JSON payload from the event
- Posts a BBj custom event using `BBjAPI().postPriorityCustomEvent()` with the action name and payload

This allows BBj to receive and process events sent from web clients via the WebguiBridge.

## Sample

```java
use ::BBjWebguiBridge/BBjWebguiBridge.bbj::BBjWebguiBridge
use com.google.gson.JsonObject

class public Sample
    method public Sample()
        declare BBjWebguiBridge bridge!
        declare JsonObject@ eventData!

        rem Initialize BBjWebguiBridge
        bridge! = new BBjWebguiBridge()
        bridge!.setCallback("onUserAction", #this!, "handleUserAction")   
        
        eventData! = new JsonObject@()
        eventData!.addProperty("message", "BBj is ready")
        bridge!.postCustomEvent("bbj_ready", eventData!)
    methodend

    method public void handleUserAction(BBjCustomEvent ev!)
        json! = CAST(JsonObject@, ev!.getObject())
        userName$ = json!.get("userName").getAsString()
        print "User action received from: " + userName$
    methodend
classend

sample! = new Sample()
process_events
```

## Dependencies

- BASIS BBj
- BASIS WebguiBridge Java library (`lib/webgui-bridge-25.10-SNAPSHOT.jar`)