# gl-file-server-client

### config

```javascript
var FileServerClient = require("gl-file-server-client");
var fsc = new FileServerClient({
    host: 'fs.msda.ge',
    port: '80',
    path: '/api/',
    key : "myPrivateKey" //optional
});
```

#### save file 
**to save file you need to have a stream**

```javascript
var fs = require("fs");
var fileName = "header.png";
var stream = fs.createReadStream(fileName);
//@key is optional
fsc.saveFile(stream, "myOtherPrivateKey", function(err, res) {
   console.log(err, res);
});
```


#### get file 

``` key ``` **is optional**

```javascript
fsc.getFile({fileId: "55d3081a6681352f129872b7", key: "myOtherPrivateKey"}, function(err, res) {
    if (err) {
        console.log(err);
        return;
    }
    res.stream.pipe(fs.createWriteStream('doodle'));
});
```

#### get cropped image 

**before cropping you need to add permitted resolutions:**

```javascript
fsc.addPermittedResolution(500, 1000);
fsc.addPermittedResolution(1000, null);
fsc.addPermittedResolution(null, 500);

//@key is optional
fsc.getImage({
    "fileId": "55d3081a6681352f129872b7",
    "key": "myOtherPrivateKey"
}, {
    width: 500, height: 1000, crop: "MC"
},
function(err, res) {
    if (err) {
        console.log(err);
        return;
    }
    res.stream.pipe(fs.createWriteStream('doodle'));
});
```

#### permitted crops in get image:

```JSON
{
    "Top Left"      : "TL",
    "Top Center"    : "TC",
    "Top Right"     : "TR",
    "Middle Left"   : "ML",
    "Middle Center" : "MC",
    "Middle Right"  : "MR",
    "Bottom Left"   : "BL",
    "Bottom Center" : "BC",
    "Bottom Right"  : "BR"
}
```

**you can call getFile and getImage as with a file if as a first parameter if you don't need the key**

```javascript
fsc.getFile("55d3081a6681352f129872b7", function(err, res) {
    console.log(err, res.stream)
});
```

