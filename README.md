# cloverx-doc
🍀convert cloverx api definition into swagger specific format

## Usage
Install
```javascript
npm i cloverx-doc --save
```
The baseDir shoud have follow directory struct.
```shell
.
├── controller # Your api definitions must be created in this folder.
│   └── v1
│       ├── bundle.js
│       └── deep
│           └── client.js
└── schema
    └── swagger
        └── definitions.js # Your data model definations are here
```
then comment your api following jsdoc style
```javascript
/**jsdoc
 * The description of your api
 * @tags client, cli
 * @httpMethod post
 * @path /bundle/:platform
 * @param {string#path} platform - description
 * @param {string#formData} dependencies - description, support markdown
 * ```javascirpt
 * Example
 * {
 *   "dfc": "~1.1.0"
 * }
 * ```
 * @response [@Module]
 */
function dependencies() {

}
```
and finally
```javascript
const cloverxDoc = require('cloverx-doc');
let output = cloverxDoc.convert({
    baseDir: baseDir
    config: {
        host: '127.0.0.1:7078', // server address
        schemes: ['http'], // protocol, optional: https, http
        basePath: '/', // the prefix of url path
        // info about your app
        info: {
            version: '1.0.1',
            title: 'clover doc test',
            description: 'from test'
        }
    }
});
```
the swaggerDoc which generated by above code can be see [here](https://github.com/clover-x/cloverx-doc/blob/master/test/fixtures/swagger-doc.json)

## Comment Style
The wraper must start with `/**jsdoc`
```javascript
/**jsdoc
 * The description of your api write at start of the comment body.
 * markdown support
 * @tag ...
 * @tag ...
 */
```
## Tag Defination
### tags
which collection that api was belonged to.  
the tags was defined in `schema/swagger/tags.yaml`.  
Example: `@tags client, cli`

### httpMethod
The action of http request which in lowercae.  
Example: `@httpMethod post`

### path
The path part of url, `Fianl Path = basePath + directory path + path`

### param
`@param {data type#location} params name - comment`  
Avaliable location see [here](http://swagger.io/specification/#fixed-fields-45).  
Avaliable data type see [here](http://swagger.io/specification/#data-types-12)

### response
The Modules define in `schema/swagger/definitions.js`.  
ref module directly like `@ModuleName`.  
wrap moudel in an array like `[@ModuleName]`.

## Reference
* [usejsdoc](http://usejsdoc.org/)
* use [swagger editor](http://editor.swagger.io/#/) to visualize the output of cloverx-doc

