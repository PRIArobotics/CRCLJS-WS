# CRCLJS-WS

Browser Websocket implementation of the queued streaming robot motion interface [CRCL-JS](https://github.com/PRIARobotics/CRCLJS), an adapted minimal JSON-based version of [CRCL](https://github.com/usnistgov/crcl/blob/master/doc/Reference.md).

There are also two specific implementations:

- a general Javascript implementation with the reference: [CRCL-JS](https://github.com/PRIARobotics/CRCLJS)
- a Node-JS specific implementation: [CRCL-JS-Node](https://github.com/PRIARobotics/CRCLJS-Node)


### CRCLJS Websocket Communication Interface

The implemented [SocketIORobotConnection](https://github.com/PRIArobotics/CRCLJS-WS/blob/main/src/SocketIORobotConnection.js) can connect to the websocket of the [CRCLJS WS Adapter](https://github.com/PRIARobotics/CRCLJS-WSAdapter) which forwards the CRCLJS commands to the robots. This enables CRCLJS robot communication in the browser since it does not depend on TCP.

## Private Dependencies

Use `npm link` to resolve the following private dependencies:

* [CRCLJS](https://github.com/PRIARobotics/CRCLJS)
* [eulerutil](https://github.com/PRIARobotics/eulerutil)

## Usage

```javascript
import {Tools} from "@babylonjs/core";
import {CommandFactory, RobotInterface} from 'crcljs';
import {quaternionToEuler} from 'eulerutil';
import {SocketIORobotConnection} fron './src/SocketIORobotConnection.mjs'

const ROBOTSERVER = 'http://localhost:3000'
console.log(`Connecting to crcljs ws adapter at ${ROBOTSERVER}`);
const robotServerConnection = io(ROBOTSERVER, {reconnection: false});

robotServerConnection.on('connect', () => {
    console.log(`Connected to RobotServer`)
    const robotname = 'Kuka'
    const robotInterface = new RobotInterface(new SocketIORobotConnection(robotname, robotServerConnection))
    if (!robotInterface.connected){
        console.log("RobotInterface disconnected")
        return
    }
    try{
        this.robotInterface.send(CommandFactory.SetEndEffectorParameters("Using Gripper 0", 0))
        this.robotInterface.send(CommandFactory.SetTransSpeed('Set speed to 10 percent', 0.1)
        
        const rotation = quaternionToEuler(myrotationQuaternion, 'XYZ').asArray().map(Tools.ToDegrees).reverse()
        const position = newtcp.position.scale(1000).asArray() // meter to millimeter
        const cmd = CommandFactory.MoveTo('Moving', position, rotation, false, false)
        
        const [queued, working, done] = robotInterface.send(cmd)
        await queued
        await working
        await done
    } catch (e){
        console.log(e.message)
    }
}
```
