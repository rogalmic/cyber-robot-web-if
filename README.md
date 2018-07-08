# cyber-robot-web-if
[Clementoni Cyber Robot](https://www.google.pl/search?q=Clementoni+cyber+robot&source=lnms&tbm=isch) web access via Google Chrome's Web Bluetooth experimental feature.

Some info about protocol - [CyberRobotBrain java project](https://github.com/prof18/CyberRobotBrain/tree/master/app/src/main/java/com/clemgmelc/cyberrobotbrain)

Some bluetooth Gatt info about the device:

```
BTHLEDevice\{00001800-0000-1000-8000-00805f9b34fb}_Dev_VID&0100c0_PID&0000_REV&0000_1b7e9706073d\9&8c643bc&0&0001
BTHLEDevice\{00001801-0000-1000-8000-00805f9b34fb}_Dev_VID&0100c0_PID&0000_REV&0000_1b7e9706073d\9&8c643bc&0&000c
BTHLEDevice\{0000180a-0000-1000-8000-00805f9b34fb}_Dev_VID&0100c0_PID&0000_REV&0000_1b7e9706073d\9&8c643bc&0&0010
BTHLEDevice\{0000feba-0000-1000-8000-00805f9b34fb}_Dev_VID&0100c0_PID&0000_REV&0000_1b7e9706073d\9&8c643bc&0&0027
BTHLEDevice\{0000fff0-0000-1000-8000-00805f9b34fb}_Dev_VID&0100c0_PID&0000_REV&0000_1b7e9706073d\9&8c643bc&0&0047
BTHLEDevice\{0000fff3-0000-1000-8000-00805f9b34fb}_Dev_VID&0100c0_PID&0000_REV&0000_1b7e9706073d\9&8c643bc&0&0051
```

```
Requesting any Bluetooth Device...
Connecting to GATT Server...
Getting Services...
Getting Characteristics...
> Service: 00001800-0000-1000-8000-00805f9b34fb
>> Characteristic: 00002a00-0000-1000-8000-00805f9b34fb [READ]
>> Characteristic: 00002a01-0000-1000-8000-00805f9b34fb [READ]
>> Characteristic: 00002a02-0000-1000-8000-00805f9b34fb [READ]
>> Characteristic: 00002a04-0000-1000-8000-00805f9b34fb [READ]
> Service: 00001801-0000-1000-8000-00805f9b34fb
>> Characteristic: 00002a05-0000-1000-8000-00805f9b34fb [READ, INDICATE]
> Service: 0000180a-0000-1000-8000-00805f9b34fb
>> Characteristic: 00002a24-0000-1000-8000-00805f9b34fb [READ]
>> Characteristic: 00002a29-0000-1000-8000-00805f9b34fb [READ]
>> Characteristic: 00002a23-0000-1000-8000-00805f9b34fb [READ]
>> Characteristic: 00002a26-0000-1000-8000-00805f9b34fb [READ]
>> Characteristic: 00002a27-0000-1000-8000-00805f9b34fb [READ]
>> Characteristic: 00002a28-0000-1000-8000-00805f9b34fb [READ]
>> Characteristic: 00002a50-0000-1000-8000-00805f9b34fb [READ]
> Service: 0000feba-0000-1000-8000-00805f9b34fb
>> Characteristic: 0000fa12-0000-1000-8000-00805f9b34fb [READ]
>> Characteristic: 0000fa10-0000-1000-8000-00805f9b34fb [WRITEWITHOUTRESPONSE, NOTIFY]
>> Characteristic: 0000fa11-0000-1000-8000-00805f9b34fb [WRITE, INDICATE]
> Service: 0000fff3-0000-1000-8000-00805f9b34fb
>> Characteristic: 0000fff4-0000-1000-8000-00805f9b34fb [NOTIFY]
>> Characteristic: 0000fff5-0000-1000-8000-00805f9b34fb [WRITEWITHOUTRESPONSE] // MOVEMENT CHARACTERISTICS HERE
> Service: 0000fff0-0000-1000-8000-00805f9b34fb
>> Characteristic: 0000fff1-0000-1000-8000-00805f9b34fb [NOTIFY]
>> Characteristic: 0000fff6-0000-1000-8000-00805f9b34fb [NOTIFY]
>> Characteristic: 0000fff2-0000-1000-8000-00805f9b34fb [WRITE, NOTIFY]
```

Kindof-working communication sample (after enabling experimental feature in chrome):
```
navigator.bluetooth.requestDevice({ filters: [{ services: ['0000fff3-0000-1000-8000-00805f9b34fb'] }] })
.then(device => device.gatt.connect())
.then(server => server.getPrimaryService('0000fff3-0000-1000-8000-00805f9b34fb'))
.then(service => service.getCharacteristic('0000fff5-0000-1000-8000-00805f9b34fb'))
.then(characteristic => {
  var forwardCommand = Uint8Array.of(0x31, 0x32, 0x44, 0x30, 0x53, 0x2d, 0x31);
  return characteristic.writeValue(forwardCommand);
})
.then(_ => {
  console.log('Command sent.');
})
.catch(error => { console.log(error); });
```
