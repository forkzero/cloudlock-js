[![Actions Status](https://github.com/forkzero/cloudlock-js/workflows/Node%20CI/badge.svg)](https://github.com/forkzero/cloudlock-js/actions)

# cloudlock-js
*** coming soon, if you need this please email gmoon@forkzero.com ***

JavaScript client for the FORKZERO Cloud Lock service

### Use Cases
* Lock/unlock for exclusive access to a resource for distributed, uncoordinated clients

### Install
`npm install cloudlock --save`

### Use
`// es6
import CloudLock from 'cloudlock'
const resource = new CloudLock('my-resource')`

### Lock for Distributed Computing
Obtain a lock
```javascript
const resource = new CloudLock('myresource');
resource.lock((lock => {
  if (resource.locked) {
    // do stuff
    resource.unlock();
  } 
});
```

Obtain a lock with progressive retries, up to timeout
```javascript
const resource = new CloudLock('s3:my-bucket/my-file');
resource.wait()
  .then(lock=>{
    if (resource.locked) {
      // do stuff
      resource.unlock();
    }
  })
  .catch(error => {
    // unable to obtain lock before timeout
  })
```

Event-based Interface
```javascript
const resource = new CloudLock('my-business-process');
resource.wait();

resource.on('lock', lock => {
  if (resource.locked) {
    // do stuff
  }
  resource.unlock();
})

resource.on('timeout', () => {
  // want to try again?
  resource.wait();
})
```

### Check to see if a resource is locked
```javascript
const resource = new CloudLock('my-resource');

if (!resource.status().locked) {
  // resource is not locked; proceed
}
```
