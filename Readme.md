
# mongoose-stamp

Simple plugin for [Mongoose](https://github.com/LearnBoost/mongoose) which adds `createdAt`, `updatedAt`, and `deletedAt` date attributes
that get auto-assigned to the most recent create/update timestamp. Currently, you need to manually manage `deletedAt`.

## Installation

`npm install --save mongoose-stamp`

## Usage

```javascript
var stampIt = require('mongoose-stamp');
var userSchema = new Schema({
    username: String
});
UserSchema.plugin(stampIt);
mongoose.model('User', userSchema);
var User = mongoose.model('User', userSchema)
```
The User model will now have `createdAt` and `updatedAt` properties, which get
automatically generated and updated when you save your document. mongoose-stamp will also add support for developers setting the `deletedAt` property on a model instead of removing it. However, due to limitations of the mongoose plugin architecture, we are unable to automaticlly add support for the `deletedAt` property. mongoose-stamp requires developers to maintain the `deletedAt` property themselves if they use it.

```javascript
var user = new User({username: 'Prince'});
user.save(function (err) {
  console.log(user.createdAt); // Should be approximately now
  console.log(user.createdAt === user.updatedAt); // true
  // Wait 1 second and then update the user
  setTimeout( function () {
    user.username = 'Symbol';
    user.save( function (err) {
      console.log(user.updatedAt); // Should be approximately createdAt + 1 second
      console.log(user.createdAt < user.updatedAt); // true
    });
  }, 1000);
});
```

## License

(The MIT License)

Copyright (c) 2013-2016 Tarun Chaudhry &lt;opensource@chaudhry.co&gt;

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
