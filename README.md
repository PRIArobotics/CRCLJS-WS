# CRCLJS-WS
CRCLJS Websocket Communication Interface

## Usage

Pseudo Code example:

```javascript
import {Tools} from "@babylonjs/core";
import {CommandFactory, RobotInterface} from 'crcljs';
import {quaternionToEuler} from 'eulerutil';

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
