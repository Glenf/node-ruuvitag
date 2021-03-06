# node-ruuvitag
Node.js module for reading data from a [Ruuvitag](http://tag.ruuvi.com)
weather station.

Tested on Raspberry Pi 3. Depends on [noble](https://github.com/sandeepmistry/noble). See [instructions](https://github.com/sandeepmistry/noble) on
 how to enable BLE on RasPi and how to run without root.

### Installation

```
npm install node-ruuvitag --production
```


### Usage example
```
const ruuvi = require('node-ruuvitag');

ruuvi.on('found', tag => {
  console.log('Found RuuviTag, id: ' + tag.id);
  tag.on('updated', data => {
    console.log('Got data from RuuviTag ' + tag.id + ':\n' +
      JSON.stringify(data, null, '\t'));
  });
});

```

### Events

Module ```ruuvi``` emits a ```found``` event, when a new RuuviTag
is discovered. Event's payload is a ```ruuviTag``` object (see below)

### API

##### ruuvi.findTags()

Finds available ruuvitags. Returns a promise which is resolved with an
array of ```ruuviTag``` objects or rejected with an error if no tags were
found.

If you call ```findTags``` multiple times, it always returns **all**
found RuuviTags this far.

### ```ruuviTag``` object

Is an ```eventEmitter``` .

**Properties:**

```id```: id of beacon

**Events:**

```updated```: emitted when weather station data is received.
Object ```data``` has
following properties (depending on data format):

* ```url``` -- original broadcasted url if any
* ```temperature```
* ```pressure```
* ```humidity```
* ```eddystoneId``` -- in data format 4
* ```rssi```
* ```battery``` (battery voltage)
* ```accelerationX```
* ```accelerationY```
* ```accelerationZ```

See [data formats](https://github.com/ruuvi/ruuvi-sensor-protocols) for
info about RuuviTag sensor values.



