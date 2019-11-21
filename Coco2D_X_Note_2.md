> Coco2D-X Note 2

# Event

## Listener

- EventListenerTouch
- EventListenerKeyboard
- EventListenerAcceleration
- Event==Listen==Mouse
- EventListenerCustom (for customized event)

### swallowing an event

_when we have listened to an event, then we should swallow it, instead of getting it listened to by the next listeners_

## priority

*which listener firstly hear an event*

- if set priority value, bigger first, smaller second
- if pointed to a node, see the Scene Graph, reverse of the node's priority

> here skipped the TouchListener chap

## keyboard event

